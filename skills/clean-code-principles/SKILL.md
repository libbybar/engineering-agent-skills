---
name: clean-code-principles
description: Use when writing, changing, refactoring, or reviewing code in any language. Covers clear names, focused responsibilities, abstraction, dependencies, comments, and keeping changes scoped.
---

# Clean Code Principles

Write code that the next person can understand and change with confidence. Apply these principles to the code you work on. Follow the project's conventions when they are more specific.

## Names and structure

- Name things for what they mean in the domain and what they do. A short, ordinary name is enough when the context makes its meaning clear.
- Name a literal value when the name adds meaning; keep obvious values inline.
- Give each function a clear job. Keep its steps at a similar level of abstraction, so the reader can follow the intent without jumping between high-level decisions and implementation details.
- Extract a function when its name makes the surrounding code easier to read or when the operation has a useful boundary of its own.
- Keep related behavior together. Separate responsibilities when they have different reasons to change.
- Make state changes and other side effects clear to callers. A function's name and contract should give a fair picture of its behavior.

## Reuse and dependencies

- Check how the project already solves similar problems. Reuse or extend existing code when it fits; otherwise, follow the project's established patterns.
- Keep dependencies clear and as narrow as the task allows. Put a boundary between business rules and outside systems when that boundary makes the code easier to change or test.
- Keep one clear source of truth for rules and knowledge that must stay in sync. A little repeated syntax is often easier to understand than a shared abstraction that joins unrelated things.
- Choose the simplest structure that explains the behavior well today. Add layers or generalize when there is a concrete need.

## Comments and documentation

- Use names and structure to explain what the code does. Use comments to explain what the code alone cannot tell the reader.
- Preserve comments that explain a surprising decision, an invariant, an external constraint, a safety or reliability concern, or a workaround and when it can be removed.
- Explain subtle test setups, races, and seemingly odd code when a future change could easily break them.
- Write comments about the current behavior.
- Keep comments near changed code accurate, and document public APIs according to the project's conventions.
- State the reason plainly. For example: "Changing this array in place breaks undo." The explanation matters more than an attention-grabbing label.

## Scope and finishing

- Make the changes needed for the task and the nearby changes needed to keep the result correct and coherent.
- Remove debug code, dead branches, and unused pieces introduced or made obsolete by your change.
- When the project has tests, add regression coverage for fixed bugs. Follow the project's testing conventions.
- Review the result from the caller's point of view. Each change should make behavior, readability, correctness, testing, or maintenance better—not merely satisfy a style rule.
