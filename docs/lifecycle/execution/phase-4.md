# docs/lifecycle/execution/phase-4.md — Phase 4 Execution Protocol

## docs/lifecycle/execution/phase-4.md — Phase Name

Phase 4 — Architecture, Release Contract, DevOps, Versioning

## docs/lifecycle/execution/phase-4.md — Purpose

Turn approved discovery, scope, and state into a buildable operating model by defining:

- architecture,
- release contract,
- DevOps bootstrap,
- versioning boundaries,
- and durable human-only operational dependencies.

## docs/lifecycle/execution/phase-4.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-4.md`
- `docs/product/problem.md`
- `docs/product/scope.md`
- `docs/maintainers/state-contract.md`
- `docs/maintainers/architecture.md`
- `docs/maintainers/release-contract.md`
- `docs/maintainers/devops.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/human-actions.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-4.md — Preconditions

- `current_phase` must be `4`.
- Phases 1 through 3 must already be approved.

## docs/lifecycle/execution/phase-4.md — AI Operating Mode

Use:

- propose,
- synthesize,
- constrain,
- critique,
- verify.

## docs/lifecycle/execution/phase-4.md — Question Batch

Ask only when needed. If needed, ask one full batch covering:

- What architecture shape best fits the state contract?
- What concerns must remain delegated to the host platform?
- What repository and PR workflow is required?
- How will releases be tagged and published?
- What environments and CI/CD behaviors exist?
- What are the major/minor/patch boundaries for this project? Patch: no durable state change, no observable behavior change. Minor: additive durable state only — new obligations created, none broken. Major: existing durable state obligation changed or removed — migration path required before implementation.
- What durable human-only actions are required for this project?
- How should the AI verify those human-only actions when they occur?

## docs/lifecycle/execution/phase-4.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-4.md`
- `docs/maintainers/architecture.md`
- `docs/maintainers/release-contract.md`
- `docs/maintainers/devops.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/human-actions.md`

## docs/lifecycle/execution/phase-4.md — What To Write

Write current truth only.

`docs/maintainers/architecture.md` must contain:

- high-level architecture,
- component boundaries,
- delegated concerns,
- interface assumptions,
- fit to state contract.

`docs/maintainers/release-contract.md` must contain:

- branch and PR rules,
- merge rules,
- release-tagging rules,
- publication rules,
- environment expectations.

`docs/maintainers/devops.md` must contain:

- new-repo bootstrap,
- CI/CD expectations,
- environment setup expectations,
- operational prerequisites,
- what the AI can verify directly versus what requires human action.

`docs/maintainers/versioning-boundaries.md` must contain:

- what counts as patch — no durable state change, no observable behavior change,
- what counts as minor — additive durable state changes only; new obligations must be recorded in the state contract at the time they ship,
- what counts as major — any change that breaks or removes an existing durable state obligation; migration path must be defined before implementation begins,
- how state and migration changes affect versioning.

`docs/maintainers/human-actions.md` must contain a durable catalog of project-specific human-only actions, including:

- action name,
- why the human must do it,
- when it is needed,
- how the AI should check completion,
- what evidence counts as sufficient,
- what to do if the action cannot be verified,
- what contract area is affected if the human environment changes.

`docs/lifecycle/state/phase-4.md` must contain:

- current status,
- architecture and release summary,
- unresolved questions,
- linked durable artifacts,
- approval section,
- reopen notice section when needed,
- next-step note.

## docs/lifecycle/execution/phase-4.md — Human Action Contract Rule

Any time the human's operating environment changes in a way that may invalidate current assumptions, the AI must evaluate which contract area is affected:

- reopen Phase 4 if DevOps, release workflow, versioning workflow, or durable human-only operational dependencies are invalidated,
- reopen Phase 3 if durable-state, persistence, or migration assumptions are invalidated,
- reopen Phase 2 if scope or non-goals are invalidated,
- reopen Phase 1 if the user or problem framing is invalidated.

## docs/lifecycle/execution/phase-4.md — Blocking Points

Stop if:

- architecture or release questions need human answers,
- architecture conflicts with the state contract,
- release rules are not explicit enough,
- the durable catalog of human-only actions is incomplete,
- lifecycle corruption is detected.

## docs/lifecycle/execution/phase-4.md — Approval Gate

When all owned durable artifacts are coherent and aligned, ask directly:

> To approve this phase, reply exactly: `Approve Phase 4`

## docs/lifecycle/execution/phase-4.md — On Approval

On exact approval:

- mark Phase 4 approved in `docs/lifecycle/state/phase-4.md`,
- set `current_phase: 5`,
- set `current_state: "ready_to_execute"`,
- set `blocking_now: false`,
- set `blocking_reason: ""`,
- set `stop_reason: "Phase 4 approved. Advanced to Phase 5."`,
- set `next_required_human_action: "Review and approve the initial implementation plan before implementation begins."`,
- set `expected_human_reply: null`,
- set `return_to_phase: null`,
- set `current_execution_protocol: "docs/lifecycle/execution/phase-5/entrypoint.md"`,
- set `current_phase_artifact: "docs/lifecycle/state/phase-5.md"`,
- set `linked_artifacts` to:
  - `docs/todos.md`
  - `docs/maintainers/implementation-plan.md`
  - `docs/maintainers/architecture.md`
  - `docs/maintainers/state-contract.md`
  - `docs/maintainers/versioning-boundaries.md`
  - `docs/maintainers/human-actions.md`.

## docs/lifecycle/execution/phase-4.md — On Reopen

If later contradiction or human request forces reopening Phase 4:

- add a reopen notice and cleanup plan to the top of `docs/lifecycle/state/phase-4.md`,
- summarize what invalidated architecture, release, DevOps, versioning, or human-action assumptions,
- summarize what downstream cleanup is required,
- mark affected later-phase durable docs invalid at the top,
- stop for explicit human approval before continuing.
