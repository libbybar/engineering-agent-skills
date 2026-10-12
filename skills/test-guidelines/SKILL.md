---
name: test-guidelines
description: Use whenever writing, reviewing, or reasoning about tests (unit, integration, e2e), and when reviewing or fixing a suspected bug. Covers what makes a test worth having, assertion strength, test doubles, flakiness, test scope and layering, security-boundary testing, and what to do in a codebase with no test convention yet.
---

# Test Guidelines

Tests should tell us whether the code behaves as intended. A passing suite and high coverage help, but neither tells us whether the tests can catch a bug. Check that directly by temporarily breaking the behavior a test protects.

## Core principle

Test behavior or contracts through results you can observe. Before adding a test, be able to explain what bug, regression, or boundary it protects.

## Before writing tests

1. Identify the behavior or contract the code is supposed to guarantee.
2. Identify the failure modes and edge cases that matter.
3. Decide what is observable from outside the unit under test.
4. Choose assertions that a correct implementation passes and a broken one fails.
5. Read the implementation only as far as needed to reach and exercise that behavior.
6. Add a test only where a real behavior, risk, boundary, or contract justifies it.

## What to test

Inputs and their observable outputs. State transitions. What the user sees and can do. HTTP status codes and response shapes. Persisted state, when persistence is part of the contract. Side effects that matter. Validation boundaries. Authorization and ownership rules. Error handling and dependency failures. Edge cases, including repeated or duplicate actions. Interactions between modules, when the interaction itself is the contract.

## Cases to consider

For each behavior, pick the cases that carry real risk. Don't run through the whole list by default.

- Happy path
- Invalid input, boundary values, missing data
- Dependency failure, and a realistic malformed response from the dependency: missing fields, wrong types, invalid enum values, data that is structurally valid but semantically wrong, truncated or unparsable JSON
- Unauthorized or forbidden access
- The same action performed twice
- State before and after the operation

## Keep tests about behavior

- Assert on public, observable outcomes whenever they can prove the behavior.
- Assert on internal calls only when the call itself is part of the contract.
- Assert that something did *not* happen when that absence is part of the contract.
- Write the expected value independently of the production algorithm. If the test recomputes the answer the same way the code does, it can't catch a wrong formula.
- Mock at real boundaries. Keep the unit under test real.
- Use snapshots only when they express a meaningful contract. Keep them small enough to review, read the diff before accepting an update, and add focused assertions where the behavior needs more precision.
- Coverage shows what was never exercised. It doesn't show what is worth testing; risks and contracts decide that. A green suite is only as good as the assertions in it.

→ Weak vs. strong assertions, including the security-boundary pattern: `references/assertion-examples.md`
→ Choosing between dummy, stub, fake, spy and mock: `references/test-doubles.md`

## Flakiness and determinism

An unpredictable test is hard to trust, even when it sometimes catches a real problem.

- Control time, randomness and external dependencies unless their real behavior is part of the contract being tested. Unit tests use deterministic substitutes; integration and e2e tests may use real dependencies to prove the integration. Keep their state isolated and failures diagnosable.
- Don't add a sleep to hide a race condition. Wait for the actual condition or event. Keep asynchronous waits bounded; set an explicit timeout when the test runner's default doesn't suit the operation.
- An intermittent failure is a bug to diagnose, not a test to retry or delete. Check for hidden order dependence (see Test independence).
- If the system is eventually consistent by design, poll for the final state with an explicit time limit instead of a fixed sleep.

## Test speed

Keep fast tests fast. Move expensive checks to the layer where they belong.

- Unit tests stay fast: no real database, network, filesystem, or sleep.
- Tests with expensive setup (real database, real HTTP, containers) go in a separate integration or e2e layer.
- Reuse setup when it's cheap and doesn't break independence. Let isolated tests run in parallel.

## Test scope and layering

Put each test in the cheapest layer that can still prove the behavior.

- **Unit:** pure logic, single functions or components, edge cases and boundary values. Fast, isolated, no real I/O.
- **Integration:** real collaboration between modules, real database or framework wiring, HTTP route behavior. Use when the interaction between parts is the contract.
- **End-to-end:** full user flows through the real system. Focus on critical paths and keep them proportionate to their cost.

Don't re-prove framework or library behavior in a higher layer than needed.

When several implementations must satisfy the same interface, write one parameterized contract suite that runs unchanged against each of them and asserts only what every correct implementation must share. Implementation-specific behavior gets its own tests.

→ HTTP routes and UI components: `references/server-and-client-testing.md`

## One behavior per test

Each test covers one behavior or one reason to fail. Several assertions are fine when together they prove that one behavior. Split the test when a failure could point to unrelated causes, so that a red test tells you what broke.

## Test independence and test data

Each test should pass alone, in any order.

- Create the state a test needs inside the test or its own setup. Keep mutable state isolated.
- Reset shared stores, databases, mocks, timers, files and globals before or after each test. When investigating order-dependent failures, change or randomize the test order if the runner supports it.
- Give test data only the fields that matter to the assertion, so it's obvious what the test depends on.
- When the same shape keeps coming up, use a builder or factory instead of copy-pasted literals. Avoid shared fixtures that quietly couple tests together.
- For dependency failures, use realistic responses from that boundary. Deliberately malformed inputs should violate only the condition the test is meant to check.

## Security and privacy boundaries

When a security or privacy contract requires different failures to look the same, prove that they do. For example, if missing, expired, malformed and wrongly signed tokens must get the same generic response, cover each cause and compare the full externally visible error. Do not assume every application or every authentication failure must use the same response.

→ Worked example: `references/assertion-examples.md`

## After writing or reviewing tests

When reviewing existing tests, first identify the expected behavior and risks without relying on the scenarios the tests already cover. Look for missing cases as well as weak assertions in the tests that exist.

1. For each meaningful test, say briefly what bug, regression, boundary or contract it protects.
2. **Check that the test catches a real break.** For every new or materially changed behavioral test, introduce a temporary, targeted defect in the behavior it protects. Run the relevant test and confirm it fails **because of that defect**, not because of a setup or unrelated error. Use a mutation-testing tool when available, or make a controlled manual mutation. Multiple tests may share a mutation only when its failure genuinely verifies each one. A full mutation suite is not required for every change. Restore the original code and run the relevant tests again; they must pass. Protect existing uncommitted changes: use an isolated worktree/copy or another reversible method, and never reset or overwrite someone else's work. If the mutation check cannot be run safely, report exactly which tests were not verified and why; never claim verification that did not happen.
3. Check that mocks set up boundary conditions without deciding the answer in advance, and that assertions on internal calls exist only where the call is the contract.
4. Check that the tests would still pass after a harmless refactor.
5. Check that the failure paths that matter are covered, including any required "this must not happen" assertions.
6. Check that tests are independent of each other and of run order.
7. Run the relevant tests with the correct implementation restored, then run the surrounding suite when available.
8. If some important behavior is hard to test reliably because of the current design, say so and explain why.

→ Investigating or fixing a bug: `references/bug-workflow.md`

## Test cost

Tests need maintenance too. Add them because they catch something worth protecting.

- Don't prove the same thing at two layers unless the higher layer adds real evidence.
- Don't test trivial code (plain getters, generated boilerplate, pass-through delegation) where a bug is implausible.
- Fewer strong tests beat many shallow ones that all fail together for the same reason.
- If a harmless refactor breaks many tests for the same reason, the tests are coupled to implementation details. Fix the coupling instead of updating each test.
- Delete or merge tests that no longer protect a real risk, boundary or contract.

## Testability and architecture

Production design serves the product first, but important behavior should stay observable through clean boundaries. When the current design makes important behavior hard or impossible to test:

1. Describe the behavior that needs reliable coverage.
2. Explain the testability problem.
3. Name the architectural cause.
4. Propose the smallest change that fixes it.
5. Get approval before architectural changes or changes outside the task's scope made only for testability. Small, behavior-preserving changes within the task's scope do not need separate approval.

## Working in an existing codebase

Read the existing tests first and match their tooling, syntax, naming, setup patterns and global conventions. Apply the principles above within those conventions.

If there are no tests or no clear convention, **don't pick a framework just because it's typical for the stack.**

→ The order in which to look for evidence, and what to do if there is none: `references/codebase-conventions.md`

## Reference files

Read these only when the situation calls for them:

- `references/test-doubles.md`: definitions of dummy, stub, fake, spy and mock, and when to use each. Read when writing or reviewing a test double.
- `references/assertion-examples.md`: weak vs. strong assertions, and the security-boundary token example. Read when judging how strong an assertion is, or when testing a security boundary.
- `references/server-and-client-testing.md`: specifics for HTTP routes and UI components. Read when the code under test is a route or a component.
- `references/bug-workflow.md`: how to review a suspected bug and how to fix a confirmed one. Read when investigating or fixing a bug.
- `references/codebase-conventions.md`: how to find the project's test framework and conventions when none is obvious. Read at the start of a project, or when there are no tests yet.
