# Engineering Agent Skills

Skills I use with coding agents to set an engineering standard for the code they write, change, and review.

Agents can already write working code. What I want from them is code that's easy to read and change, failures that are handled on purpose, and tests that actually catch broken behavior. These skills describe that standard in a way the agent can apply with judgment, alongside each project's own conventions.

## Skills

| Skill | Use it for |
| --- | --- |
| [`clean-code-principles`](skills/clean-code-principles/) | Names, function responsibilities, reuse, abstractions, comments, and keeping changes scoped |
| [`error-handling`](skills/error-handling/) | Failure paths: who handles an error, meaningful errors, recovery, state, and reporting |
| [`test-guidelines`](skills/test-guidelines/) | Writing, reviewing, and fixing tests, including checking that a test can actually fail |

Each skill works on its own, and they also work together. When they're combined, the general skills say what a change needs to verify, and `test-guidelines` defines how to test it well.

All three are language-agnostic. They define how code is written and checked; how the result is delivered (output format, tooling, workflow) is left to the task and the environment the agent works in.

## Using them

Copy the skill folders you want into your coding agent's skills directory. For example, in Claude Code:

- `~/.claude/skills/<skill-name>/` for all your projects
- `.claude/skills/<skill-name>/` inside a specific project

Each skill folder contains a `SKILL.md`. `test-guidelines` also has a `references/` folder that the agent reads only when a situation calls for it.

Each skill also includes `agents/openai.yaml` with display metadata for OpenAI Codex. Other agents can ignore it.

## License

© 2026 Libby Bar. Licensed under [CC BY 4.0](LICENSE). You're welcome to use, adapt, and share these skills, with attribution.
