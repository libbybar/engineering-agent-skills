# No tests yet, or no clear convention

Read this before choosing a test framework in a project that has no tests, or whose existing tests don't settle the question.

The test framework is a project-wide architectural decision. Don't infer it per task, and don't guess from what is common for the stack.

Look for evidence that someone already decided, **in this order**:

1. A test-runner config file (`jest.config`, `vitest.config`, `pytest.ini`, `playwright.config` and so on).
2. A test script in the package or build manifest, even an unused one.
3. A testing library in the declared dependencies.
4. A CI configuration that runs a test command.
5. A README or contributing guide that names one.

Use consistent evidence of an existing decision. If the evidence conflicts (for example, a stale config and an active CI command), inspect what the project actually runs rather than blindly picking the first item. If no clear decision emerges, ask instead of guessing.

If there is no evidence anywhere in the project, **don't pick a framework because it's typical for the stack.** Stop and ask the person which framework to use and whether sibling projects have conventions to follow, as with any architectural decision (see "Testability and architecture" in SKILL.md).

Once the choice is made, record it in the project using its normal configuration and a small example test, so the next session can find the decision.
