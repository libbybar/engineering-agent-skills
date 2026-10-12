# Error Handling

A skill I use with coding agents when writing, changing, and reviewing code that can fail.

I wrote it because error handling is where agent code often looks fine and behaves badly. A `try/catch` that logs and moves on, a fallback that hides a real problem, an error message that loses the original cause. The code runs, but when something goes wrong, nobody can tell what happened or what was lost.

The idea behind this skill is that every failure should have a clear outcome: someone handles it on purpose, the caller can tell success from failure, and the user's data and work aren't lost along the way.

## What it covers

- Deciding where a failure should be handled, and letting the rest reach the right boundary
- Validating data where its shape is uncertain, and trusting invariants the code already guarantees
- Keeping errors meaningful: original cause, useful context, no sensitive details leaking out
- Fallbacks that are valid results, not ways to hide a failure
- Preserving user input and keeping related state changes consistent
- Ownership of async and concurrent work: completion, cancellation, failure, and cleanup
- Retries and timeouts that follow the project's existing policies
- Verifying the failure behavior a change affects

The principles aren't tied to a language or framework. They work with the language's usual error mechanisms (exceptions, result types, return values) and the project's conventions.

## What it doesn't cover

The skill defines how failure paths are written and checked. How the result is delivered is decided by the task and the environment the agent works in.

## Using it

Give the agent access to `SKILL.md` by placing the `error-handling/` folder in its skills directory.

For example, in Claude Code:

- `~/.claude/skills/error-handling/` for all your projects
- `.claude/skills/error-handling/` inside a specific project

Other agents have their own setup, but the idea is the same.

It applies to ordinary changes too, not only to code that is "about" errors. It works on its own or together with [`clean-code-principles`](../clean-code-principles/) and [`test-guidelines`](../test-guidelines/).

## A note on catching errors

Catching an error isn't the same as handling it. A handler should do something useful: recover, provide an intended alternative, translate the error for a caller, or add context before passing it on. When none of those fits, the better choice is usually to let the error reach the boundary that can deal with it.

## License

[CC BY 4.0](../../LICENSE)
