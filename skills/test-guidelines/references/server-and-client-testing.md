# Server testing

Read this when the code under test is a server or an HTTP route.

- Test the public HTTP contract: the combinations of status code, response body and side effects that matter. When saving data is part of the behavior, check the saved state along with the response.
- Authorization and ownership tests should prove that access stays inside the right ownership boundary.
- For routes that depend on a provider or external service, cover the normal path with a deterministic double, then cover dependency failure and realistic malformed responses.
- Keep route tests about route behavior. Use broader integration tests when the integration itself is what you're testing.

# Client testing

Read this when the code under test is a client or UI component.

- Test what the user can see and do. Query by role, accessible name, label or visible text.
- Assert on component internals only when they are part of the public contract. Otherwise prove behavior through the rendered UI.
- Cover the states that matter: loading, error, success, redirects, disabled and enabled actions, transitions between screens.
- Mock the HTTP or service boundary and keep the component's own behavior real.
