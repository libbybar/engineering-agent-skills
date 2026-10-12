# Test Guidelines

A skill I use with coding agents when writing, reviewing, and fixing tests.

I wrote it because I wanted tests that catch broken behavior, not just tests that pass. Coverage is useful for finding gaps, but it doesn't tell me whether an assertion would catch a bug.

The part I care about most is **checking that a test can fail**. For every new or materially changed behavioral test, the agent temporarily breaks the behavior being tested (with a mutation-testing tool when one is available, or by hand), runs the test, checks that it fails for the expected reason, restores the code, and runs it again. I prefer the extra work to relying on a guess about how strong a test is.

That doesn't mean testing every line or adding tests for the sake of it. Each test should protect a behavior, boundary, or failure case worth maintaining.

## What it covers

- Choosing cases based on what can actually go wrong
- Testing observable results instead of implementation details
- Writing expected values independently of the production algorithm
- Deciding between unit, integration, and end-to-end tests
- Using test doubles without mocking away the behavior under test
- Testing authorization, errors, duplicate actions, and state changes
- Investigating suspected bugs before adding regression tests
- Checking new or changed tests against an intentionally broken implementation

The rules aren't tied to a language or framework. The examples happen to use JavaScript.

## What it doesn't do

The skill sets a testing process; it can't guarantee that every bug is covered. What gets tested still depends on understanding the code and the risks.

## Using it

Give the agent access to `SKILL.md` and the `references/` folder next to it by placing the `test-guidelines/` folder in its skills directory.

For example, in Claude Code:

- `~/.claude/skills/test-guidelines/` for all your projects
- `.claude/skills/test-guidelines/` inside a specific project

Other agents have their own setup, but the idea is the same.

It works on its own or together with [`clean-code-principles`](../clean-code-principles/) and [`error-handling`](../error-handling/).

`SKILL.md` contains the main rules. The reference files cover specific situations, and the agent reads them only when the situation calls for it:

- `assertion-examples.md` — examples of weak and strong assertions
- `bug-workflow.md` — investigating and fixing bugs
- `codebase-conventions.md` — what to do when a project has no clear testing setup
- `server-and-client-testing.md` — HTTP routes and UI components
- `test-doubles.md` — stubs, fakes, spies, mocks, and dummies

## A note on deliberate breakage

Temporary changes should be small and reversible. The agent must not overwrite uncommitted work, and must restore the original implementation after checking that the test fails. If it can't run the check safely, it has to say which tests weren't verified and why.

## License

[CC BY 4.0](../../LICENSE)
