# docs/lifecycle/execution/phase-8.md — Phase 8 Execution Protocol

## docs/lifecycle/execution/phase-8.md — Phase Name

Phase 8 — Continuity

## docs/lifecycle/execution/phase-8.md — Purpose

Audit whether the project is understandable, operable, and survivable by future humans and future AI agents.

## docs/lifecycle/execution/phase-8.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-8.md`
- `docs/maintainers/continuity.md`
- all files listed in `linked_artifacts`
- all files listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-8.md — Preconditions

- `current_phase` must be `8`, or the human must have explicitly said `Run Phase 8 audit`.

## docs/lifecycle/execution/phase-8.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-8.md`
- `docs/maintainers/continuity.md`

## docs/lifecycle/execution/phase-8.md — What To Audit

Audit at least:

- clarity of problem framing,
- scope legibility,
- state-contract legibility,
- architecture legibility,
- release and DevOps clarity,
- human-action contract clarity,
- implementation legibility,
- issue triage legibility,
- ease of handoff to a fresh AI or maintainer,
- document weight — identify any docs that have grown heavy enough that a fresh AI or human would struggle to hold them in context; recommend splitting if a document covers more than one distinct concern or has grown beyond what is comfortable to read in one pass.

## docs/lifecycle/execution/phase-8.md — Approval Gate

When the continuity audit package is coherent, ask directly:

> To approve this phase, reply exactly: `Approve Phase 8`

## docs/lifecycle/execution/phase-8.md — On Approval When Entered Normally

If `return_to_phase` is `null`, Phase 8 was entered as a normal lifecycle phase. Remain in Phase 8 until the human explicitly directs the next step.

## docs/lifecycle/execution/phase-8.md — On Approval When Called

If `return_to_phase` is set, Phase 8 was called from another phase. On approval, return to the caller unless Phase 8 revealed a contradiction requiring an earlier phase to reopen.

Set `return_to_phase: null` and restore the caller phase state as follows:

**Returning to Phase 6:**
- `current_phase: 6`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `stop_reason: "Phase 8 audit complete. Returning to Phase 6."`
- `next_required_human_action: "Continue Phase 6 release decisioning."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-6.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-6.md"`
- `linked_artifacts: ["docs/maintainers/release-decision.md", "docs/maintainers/versioning-boundaries.md", "docs/maintainers/state-contract.md", "docs/maintainers/human-actions.md"]`

**Returning to Phase 7:**
- `current_phase: 7`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `stop_reason: "Phase 8 audit complete. Returning to Phase 7."`
- `next_required_human_action: "Continue Phase 7 maintenance. Describe the next change or improvement."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-7.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-7.md"`
- `linked_artifacts: ["docs/maintainers/maintenance.md", "docs/maintainers/human-actions.md", "docs/maintainers/versioning-boundaries.md", "docs/maintainers/state-contract.md"]`
