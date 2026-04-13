# docs/lifecycle/execution/phase-6.md — Phase 6 Execution Protocol

## docs/lifecycle/execution/phase-6.md — Phase Name

Phase 6 — Release Decision

## docs/lifecycle/execution/phase-6.md — Purpose

Classify the current change set, determine release candidacy, and prepare the project for an actual human release action if appropriate.

## docs/lifecycle/execution/phase-6.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-6.md`
- `docs/maintainers/release-decision.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/state-contract.md`
- `docs/maintainers/human-actions.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-6.md — Preconditions

- `current_phase` must be `6`.
- Phase 5 must already be approved.

## docs/lifecycle/execution/phase-6.md — AI Operating Mode

Use:

- synthesize,
- constrain,
- verify,
- propose.

## docs/lifecycle/execution/phase-6.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-6.md`
- `docs/maintainers/release-decision.md`

## docs/lifecycle/execution/phase-6.md — What To Write

`docs/maintainers/release-decision.md` must contain:

- change inventory,
- proposed version classification,
- state-impact classification,
- migration requirement decision,
- release-candidate judgment,
- required human actions before release,
- how those human actions will be verified.

`docs/lifecycle/state/phase-6.md` must contain:

- current status,
- release decision summary,
- unresolved questions,
- linked durable artifacts,
- approval section,
- reopen notice section when needed,
- next-step note.

## docs/lifecycle/execution/phase-6.md — Change Set Reference

The change set being classified is everything implemented since the last release. For a first release, this is the entire implementation. For subsequent releases, read `docs/maintainers/release-decision.md` from the previous release to determine the baseline — the change set is everything implemented since that release was shipped.

## docs/lifecycle/execution/phase-6.md — Valid Commands In This Phase

The following lifecycle commands are valid while in Phase 6:

- `Approve Phase 6` — accepts the version classification and release candidate decision, advances to Phase 7.
- `Reopen Phase 5` — if the release candidate reveals implementation is not yet sufficient.
- `Reopen Phase 3` — if release review reveals an unrecorded durable state obligation.
- `Reopen Phase 4` — if release review reveals a versioning or DevOps contract gap.
- `Run Phase 8 audit` — callable before release if survivability concerns exist.

The following are not valid from Phase 6:
- `Reopen Phase 6` — you are already in Phase 6.
- `Reopen Phase 7` — Phase 7 has not started yet.
- `Reopen Phase 8` — Phase 8 is callable, not a reopen target.

## docs/lifecycle/execution/phase-6.md — Human Action Gates

When release candidacy is approved but an actual human release action is required, the AI must:

1. name the exact action, such as PR merge, tag creation, or publication,
2. reference the relevant entry in `docs/maintainers/human-actions.md`,
3. state exactly how it will verify completion,
4. stop with `current_state: waiting_for_human_action` until the action is verifiable.

## docs/lifecycle/execution/phase-6.md — Approval Gate

When release classification and release package are both coherent, ask directly:

> To approve this phase, reply exactly: `Approve Phase 6`

This approval means the human accepts both the version classification and the release candidate decision.

## docs/lifecycle/execution/phase-6.md — On Approval

On exact approval:

- mark Phase 6 approved in `docs/lifecycle/state/phase-6.md`,
- set `current_phase: 7`,
- set `current_state: "ready_to_execute"`,
- set `blocking_now: false`,
- set `blocking_reason: ""`,
- set `stop_reason: "Phase 6 approved. Advanced to Phase 7."`,
- set `next_required_human_action: "Review incoming maintenance work or answer maintenance questions if the AI asks them."`,
- set `expected_human_reply: null`,
- set `current_execution_protocol: "docs/lifecycle/execution/phase-7.md"`,
- set `current_phase_artifact: "docs/lifecycle/state/phase-7.md"`,
- set `linked_artifacts` to:
  - `docs/maintainers/maintenance.md`
  - `docs/maintainers/human-actions.md`
  - `docs/maintainers/versioning-boundaries.md`
  - `docs/maintainers/state-contract.md`.

## docs/lifecycle/execution/phase-6.md — Callable Phase 8 Trigger

Phase 6 may call Phase 8 if the human says `Run Phase 8 audit` or if release survivability concerns justify an audit before release.

When calling Phase 8, set:
- `current_phase: 8`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `return_to_phase: 6`
- `stop_reason: "Phase 8 audit called from Phase 6."`
- `next_required_human_action: "Continue the Phase 8 continuity audit."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-8.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-8.md"`
- `linked_artifacts: ["docs/maintainers/continuity.md"]`
