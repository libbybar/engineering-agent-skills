---
name: error-handling
description: Use when writing, changing, or reviewing runtime failure paths in any language: input and parsing, files, storage, external services, concurrent work, recovery, error reporting, and state changes.
---

# Error Handling

Give failures a clear outcome. Use the language's usual error mechanisms and the project's established conventions. Apply these principles to the code you work on, including failure paths introduced by an otherwise ordinary change.

## Decide who handles a failure

- Handle a failure where the code can make a useful decision: recover, provide an intended alternative, translate it for a caller, or add context before passing it on.
- Let other failures reach the boundary responsible for them. A handler should have a purpose beyond acknowledging that an error occurred.
- Treat expected failures as part of the operation's contract. Preserve enough information for the caller to respond appropriately.
- Let unexpected failures reach a suitable reporting boundary, with useful diagnostic context and a safe outcome for the caller.
- Validate information at boundaries where its shape or meaning is uncertain, including user input, configuration, stored data, and responses from other systems. Rely on invariants that the code already establishes internally.

## Keep errors meaningful

- Use the language's normal way to represent failure: exceptions, result types, return values, or another established mechanism.
- Preserve the original cause when adding context, so the reason for the failure remains traceable.
- Include useful, safe context: which operation failed and which identifiers matter for diagnosis.
- Keep internal details and sensitive data in the appropriate diagnostic channel. Give users and callers information they can act on.
- Keep error contracts consistent wherever other code depends on them.

## Recovery and state

- Use a fallback when it is a valid result for the task, and make any important limitation clear. For example, cached data can be useful when its age is visible.
- Keep user input and recoverable work available when an operation fails. Restore or reconcile tentative changes when needed.
- Keep related state changes consistent. Use transactions when they fit; when work spans boundaries that cannot share a transaction, define how partial completion is handled.
- Give asynchronous and concurrent work a clear owner for completion, cancellation, failure, and cleanup.
- Use the project's existing timeout, cancellation, and retry policies for external work. Retry when repeating the operation is safe, with limits that fit the situation.
- Release resources and restore temporary state on every relevant exit path.

## Reporting and verification

- Record failures where they are handled or reported, with enough context to investigate them. Pass-through layers can leave reporting to that boundary.
- Show failures to users when they affect the task or data, and offer a useful next step when one exists.
- Verify the failure behavior affected by the change, such as rejected input, unavailable dependencies, partial work, repeated operations, and recovery.

## Before finishing

Check that a real failure has a useful path through the code; callers can tell success from failure; recoverable data is preserved; partial changes have a defined outcome; and diagnostics reach the right place without leaking sensitive details.
