# docs/maintainers/human-actions.md — Human-Only Actions Catalog

## docs/maintainers/human-actions.md — Purpose

This file is the durable Phase 4 output describing project-specific human-only actions that the AI cannot safely perform or verify without a human.

This is a durable catalog, not a transient queue.

If the human's operating environment changes in a way that invalidates one of these assumptions, the AI must determine which lifecycle contract is affected and reopen the appropriate phase.

## docs/maintainers/human-actions.md — Action Template

For each durable human-only action, record:

- action name,
- why the human must do it,
- when it is needed,
- how the AI should verify completion,
- what evidence counts as sufficient,
- what to do if completion cannot be verified,
- what contract area is affected if the human environment changes.


## docs/maintainers/human-actions.md — Inclusion Rule

Only include durable, project-specific human-only actions that are likely to recur or matter again.

Do not include:

- phase approvals,
- one-off chat clarifications,
- temporary reminders,
- transient implementation tasks,
- or anything that belongs only in `docs/todos.md` or a current phase artifact.

## docs/maintainers/human-actions.md — Catalog Entries

To be filled.
