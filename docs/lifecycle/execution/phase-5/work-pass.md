# docs/lifecycle/execution/phase-5/work-pass.md — Phase 5 Work Pass Protocol

## docs/lifecycle/execution/phase-5/work-pass.md — Purpose

Execute one bounded implementation pass while preserving upstream contracts and leaving the project in a resumable state.

## docs/lifecycle/execution/phase-5/work-pass.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-5.md`
- `docs/todos.md`
- `docs/maintainers/implementation-plan.md`
- `docs/maintainers/architecture.md`
- `docs/maintainers/state-contract.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/human-actions.md`

## docs/lifecycle/execution/phase-5/work-pass.md — What One Pass Must Do

A bounded Phase 5 work pass must do exactly one of the following:

- clarify implementation ambiguity,
- update the current implementation plan,
- summarize implementation progress,
- prepare or manage one sub-agent task,
- stop at a human-answer, human-approval, human-action, or reopen gate.

## docs/lifecycle/execution/phase-5/work-pass.md — Questions

Ask questions only when implementation cannot proceed safely within the current contracts.

When needed, ask one full batch and record unresolved questions in `docs/lifecycle/state/phase-5.md`.

Delete those unresolved questions once they are resolved or explicitly discarded.

## docs/lifecycle/execution/phase-5/work-pass.md — What To Update

`docs/maintainers/implementation-plan.md` tracks total progress and the big picture. Update it when:
- a milestone is completed,
- the sequencing or scope of a milestone changes,
- a significant risk or new dependency is discovered.
It must always reflect current high-level intent. If it drifts significantly from the plan approved at Gate 1, flag it to the human before continuing.

`docs/lifecycle/state/phase-5.md` must stay concise and strategic. It should show:
- current implementation focus,
- current blocker,
- current implementation risk,
- route forward,
- temporary sub-agent notes if any.

`docs/todos.md` tracks broken down progress from the current milestone. It must stay concise and show:
- what is done,
- what not to worry about right now,
- what still needs attention,
- active workstreams (at most 3 unless the human explicitly approves more).

## docs/lifecycle/execution/phase-5/work-pass.md — Human Action Gates

If Phase 5 reaches a manual human-action step, the AI must:

1. describe the action,
2. reference the matching entry in `docs/maintainers/human-actions.md`,
3. explain how completion will be checked,
4. stop with:
   - `current_state: waiting_for_human_action`
   - `blocking_now: true`
   - `expected_human_reply: null`.

On the next pass, verify completion directly before continuing. If the human says the action is done but the AI cannot verify it, stop again and explain what evidence is missing.

## docs/lifecycle/execution/phase-5/work-pass.md — Escalation Triggers

Escalate out of normal implementation if any of the following happens:

- the work implies a state-contract change,
- the work implies a release/DevOps/versioning contract change,
- the work implies a scope change,
- the work implies a user/problem change,
- a sub-agent finds a non-straightforward issue,
- implementation reality contradicts upstream assumptions.

In those cases, route to `reopen-and-escalate.md`.
