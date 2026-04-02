## Linear Ticket Management During Implementation

When executing implementation plans that reference Linear tickets (e.g., `RIH-XXX`):

### On Starting Work

- Move referenced tickets to **In Progress** (`state: "In Progress"`) before writing any code.
- Add a comment to each ticket noting what is being worked on.

### During Work

- Add comments to the relevant ticket when a significant milestone completes (e.g., "All enums implemented", "Model tests passing").
- If blocked or if scope changes, comment on the ticket with details.

### On Completing Work

- Move the ticket to **Done** (`state: "Done"`) only after all acceptance criteria are met.
- Add a final comment summarizing what was delivered (files created, tests passing, etc.).
- If the ticket has sub-tasks or acceptance criteria checkboxes, do not mark Done until all are satisfied.

### Mapping Plan Tasks to Tickets

- Plans reference tickets in their header (e.g., `**Linear Tickets:** RIH-144, RIH-145`).
- Individual tasks within plans reference tickets in their commit messages (e.g., `feat(RIH-145): ...`).
- Use commit message references to determine which ticket a task belongs to.

### General

- Never create duplicate tickets. Search before creating.
- Always use team Rihm (`dfe15bc4-6dd0-4bde-8609-6620efc3140d`) and assign to Michael Rihm (`8d75f0a6-f848-41af-9f4b-d06036d6af82`).
- Use `save_issue` with the `id` parameter to update existing tickets, and `save_comment` to add progress notes.
