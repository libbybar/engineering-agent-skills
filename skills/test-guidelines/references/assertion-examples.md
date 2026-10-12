# Weak tests vs. strong tests

Read this when judging how strong an assertion is, or when writing a test for a security or privacy boundary. These examples apply "Keep tests about behavior" and "Security and privacy boundaries" from SKILL.md.

## Example 1: status code only vs. the full contract

Weak. It passes even if the body is wrong, the order belongs to someone else, or nothing was saved:

```js
test('creates order', async () => {
  const res = await request(app).post('/orders').send(validOrder);
  expect(res.status).toBe(201);
});
```

Stronger. It checks the body, ownership and saved state (assuming these are the route's documented promises):

```js
const validOrder = {
  items: [
    { sku: 'A1', price: 40, quantity: 2 },
    { sku: 'B2', price: 60, quantity: 1 },
  ],
};

test('creates an order owned by the authenticated user', async () => {
  const res = await request(app)
    .post('/orders')
    .set('Authorization', `Bearer ${userToken}`)
    .send(validOrder);

  expect(res.status).toBe(201);
  expect(res.body).toMatchObject({
    id: expect.any(String),
    userId: user.id,
    status: 'pending',
    total: 140,
  });

  const saved = await db.orders.findById(res.body.id);
  expect(saved.userId).toBe(user.id);
  expect(saved.total).toBe(140);
});
```

A handler that always returns 201 with an empty body passes the first test and fails the second. That is the "would a broken implementation still pass?" check in practice.

`total` is the literal `140`, not `items.reduce(...)`. If the test computed the total the same way the code does, a bug in that calculation would show up on both sides and the test would still pass.

The same idea applies to internals. A spy on `applyDiscount` passes even if the discount math is wrong, because it only proves the function was called. Mocking the unit you're testing (mocking `validateToken`, then asserting on the mock) proves nothing about the real validation. Go through the real code path and mock only the external boundary, such as the clock.

## Example 2: failure causes that must look the same (security boundary)

This applies "Security and privacy boundaries" from SKILL.md. For an API whose contract intentionally makes missing, malformed, expired and wrongly signed tokens indistinguishable:

```js
test.each([
  ['missing', undefined],
  ['malformed', 'Bearer not-a-jwt'],
  ['expired', `Bearer ${expiredToken}`],
  ['signed with the wrong secret', `Bearer ${wrongSecretToken}`],
])('%s token gets the same generic 401', async (_label, authHeader) => {
  const req = request(app).get('/account');
  if (authHeader !== undefined) req.set('Authorization', authHeader);

  const res = await req;

  expect(res.status).toBe(401);
  expect(res.body).toEqual({ error: 'Unauthorized' });
});
```

The "missing" case sends no `Authorization` header at all. Sending an empty header is a different case.

This parameterized test checks the same external contract across all causes. If a later change starts answering "token expired" for one of them, the test fails.
