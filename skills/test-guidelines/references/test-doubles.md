# Test doubles: choosing the right kind

Read this when you're about to write or review a mock, stub or fake, and need to be precise about which one it actually is.

People say "mock" for any test double at a boundary. When precision matters (in code review, when debugging a test that passes when it shouldn't, or when explaining what a test proves), tell the kinds apart:

- **Dummy:** passed only to satisfy a function signature. The code path under test never uses it.
- **Stub:** returns canned answers during the test. Use it to create inputs and boundary conditions.
- **Fake:** a lightweight working implementation, such as an in-memory database, that behaves like the real thing without the cost.
- **Spy:** a real or wrapped implementation that also records how it was called, so the test can check the interaction as well as the result.
- **Mock (strict sense):** set up in advance with the calls and arguments it expects, and fails the test itself if they don't happen. Use it when the interaction is the contract.

By default, use a stub or a fake at the boundary and assert on what is observable. Use a spy or mock only when the call itself, not just its effect, is part of what you're proving.

Mock external boundaries and expensive or non-deterministic dependencies. Configure them to create inputs and boundary conditions, and let the production code produce the result under test.
