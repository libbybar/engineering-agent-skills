# Clean Code Principles

A skill I use with coding agents when writing, changing, and reviewing code.

I wrote it because I want code that's easy to read, understand, and change later. Getting something to work is important, but so is being able to come back to it a few months later and understand what's going on.

I also found that asking an agent to write "clean code" isn't always enough. Sometimes it adds layers that aren't needed, splits simple functions into too many pieces, or creates new helpers when the project already has something that works.

This skill gives my agents a set of principles to follow, and leaves room for judgment.

## What it covers

- Choosing names that make the code easier to understand
- Keeping functions focused and responsibilities clear
- Reusing existing code and following project conventions
- Avoiding abstractions and extra layers without a real need
- Keeping dependencies and side effects understandable
- Writing comments that explain decisions, not obvious code
- Keeping changes within the task's scope
- Reviewing the result for readability and maintainability

The principles aren't tied to a language or framework. They're meant to work alongside the conventions of each project, not replace them.

## What it doesn't cover

The skill defines how code is written and changed. How the result is delivered (output format, tooling, workflow) is decided by the task and the environment the agent works in.

## Using it

Give the agent access to `SKILL.md` by placing the `clean-code-principles/` folder in its skills directory.

For example, in Claude Code:

- `~/.claude/skills/clean-code-principles/` for all your projects
- `.claude/skills/clean-code-principles/` inside a specific project

Other agents have their own setup, but the idea is the same.

It works on its own or together with [`error-handling`](../error-handling/) and [`test-guidelines`](../test-guidelines/).

The skill is useful when writing new code, changing existing behavior, refactoring, or reviewing a change.

## A note on simplicity

Clean code doesn't always mean shorter functions, fewer lines, or more reusable components.

Sometimes a little repetition is clearer than an abstraction. Sometimes keeping a few related steps together makes a function easier to follow.

I want the agent to make those decisions based on the code in front of it, rather than follow a rule just because it's there.

## License

[CC BY 4.0](../../LICENSE)
