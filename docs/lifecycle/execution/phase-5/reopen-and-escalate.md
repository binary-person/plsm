# docs/lifecycle/execution/phase-5/reopen-and-escalate.md — Phase 5 Reopen And Escalate Protocol

## docs/lifecycle/execution/phase-5/reopen-and-escalate.md — Purpose

Handle implementation findings that invalidate earlier lifecycle contracts.

## docs/lifecycle/execution/phase-5/reopen-and-escalate.md — What To Do

When implementation reveals a contradiction or the human explicitly requests a reopen:

1. Determine the earliest invalid phase.
2. Move `docs/lifecycle/state.yml` back to that phase.
3. Set `current_state` to either `reopened_due_to_contradiction` or `reopened_by_human_request`.
4. Set `blocking_now: true`.
5. Set `return_to_phase` according to the phase mapping below.
6. Write a reopen notice and cleanup plan at the top of the reopened phase artifact.
7. Mark affected later-phase durable docs invalid at the top.
8. Explain the cleanup required before continuing.
9. Stop for explicit human approval.

During the reopened phase, the AI already has prior decisions recorded in existing artifacts. Do not re-run from scratch. Check existing decisions for consistency with what triggered the reopen and only ask about genuine gaps or affected decisions. Carry forward everything still valid.

## docs/lifecycle/execution/phase-5/reopen-and-escalate.md — Phase Mapping

**Reopen Phase 4** — DevOps, release workflow, versioning, or human-only operational dependencies are invalidated:
- Set `return_to_phase: null` — Phase 4 naturally advances to Phase 5.

**Reopen Phase 3** — durable-state, persistence, or migration assumptions are invalidated:
- Set `return_to_phase: 5` — after Phase 3 is approved, skip Phase 4 and return to Phase 5.

**Reopen Phase 2** — scope or non-goals are invalidated:
- Set `return_to_phase: null` — normal downstream progression handles 2→3→4→5.

**Reopen Phase 1** — user or problem framing is invalidated:
- Set `return_to_phase: null` — normal downstream progression handles 1→2→3→4→5.
