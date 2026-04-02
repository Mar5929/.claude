## No Assumptions Policy

When writing code, updating documents, or making any change:

- **If you are not 100% certain about intent, behavior, naming, structure, or scope: ask the user before proceeding.** Do not guess, infer, or fill in gaps with assumptions.
- This applies to: ambiguous requirements, unclear variable/function names, uncertain API contracts, missing context about business logic, unclear file placement, and anything where multiple reasonable interpretations exist.
- When in doubt, state what you're unsure about and ask a specific question. One short clarifying question is always cheaper than undoing wrong work.

## Token-Heavy Operations

Before executing any action that will consume a large amount of tokens, ask the user for confirmation first. Examples:

- Reading many files or entire directories
- Large-scale refactors across multiple files
- Generating lengthy documents, specs, or reports
- Running broad codebase searches that could return massive results
- Any operation where you expect the output or input to be unusually large

State the approximate scope (e.g., "This will involve reading ~20 files" or "This refactor touches ~15 files") and get a go-ahead before proceeding.
