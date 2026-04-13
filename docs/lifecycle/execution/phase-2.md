# docs/lifecycle/execution/phase-2.md — Phase 2 Execution Protocol

## docs/lifecycle/execution/phase-2.md — Phase Name

Phase 2 — Scope

## docs/lifecycle/execution/phase-2.md — Purpose

Narrow the project into a maintainable product boundary by deciding:

- what is in scope,
- what is explicitly out of scope,
- what must be deferred,
- and what should be delegated to the host platform or other tools.

## docs/lifecycle/execution/phase-2.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-2.md`
- `docs/product/problem.md`
- `docs/product/scope.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-2.md — Preconditions

- `current_phase` must be `2`.
- Phase 1 must already be approved.

## docs/lifecycle/execution/phase-2.md — AI Operating Mode

Use:

- synthesize,
- interrogate,
- constrain,
- critique,
- verify.

## docs/lifecycle/execution/phase-2.md — Question Batch

Ask only when needed. If needed, ask one full batch covering:

- What is the minimum viable capability set?
- What is definitely out of scope?
- What should be deferred for later?
- What should be delegated to the host platform or surrounding tools?
- Which user-experience ideas would create long-term maintenance burden?
- What are the explicit non-goals?
- Are there existing docs (README, website, man page) that describe the scope? If so, should they be treated as authoritative, or are they derived from what gets defined here? If authoritative, link them from `docs/product/scope.md` rather than duplicating their content.
- Are any existing scope-related docs heavy or mixed with non-scope content? If so, discuss with the human how to restructure so there is one clear source of truth with no duplication.

## docs/lifecycle/execution/phase-2.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-2.md`
- `docs/product/scope.md`

## docs/lifecycle/execution/phase-2.md — What To Write

Write current truth only.

`docs/product/scope.md` must contain:

- in-scope capabilities,
- out-of-scope capabilities,
- deferred ideas,
- non-goals,
- maintainability constraints,
- host/platform delegation rules.

For any scope boundary where an existing doc is treated as authoritative, reference that doc rather than duplicating its content. Note that the external doc is the source of truth for that boundary.

`docs/lifecycle/state/phase-2.md` must contain:

- current status,
- resolved scope summary,
- unresolved questions,
- linked durable artifacts,
- approval section,
- reopen notice section when needed,
- next-step note.

## docs/lifecycle/execution/phase-2.md — Blocking Points

Stop if:

- scope questions need human answers,
- the human wants to trim or expand scope,
- the proposed scope conflicts with the Phase 1 problem framing,
- lifecycle corruption is detected.

## docs/lifecycle/execution/phase-2.md — Approval Gate

When the scope artifact is coherent and bounded, ask directly:

> To approve this phase, reply exactly: `Approve Phase 2`

Do not advance without that exact phrase.

## docs/lifecycle/execution/phase-2.md — On Approval

On exact approval:

- mark Phase 2 approved in `docs/lifecycle/state/phase-2.md`,
- set `current_phase: 3`,
- set `current_state: "ready_to_execute"`,
- set `blocking_now: false`,
- set `blocking_reason: ""`,
- set `stop_reason: "Phase 2 approved. Advanced to Phase 3."`,
- set `next_required_human_action: "Answer Phase 3 state-contract questions if the AI asks them."`,
- set `expected_human_reply: null`,
- set `current_execution_protocol: "docs/lifecycle/execution/phase-3.md"`,
- set `current_phase_artifact: "docs/lifecycle/state/phase-3.md"`,
- set `linked_artifacts` to `docs/maintainers/state-contract.md`.

## docs/lifecycle/execution/phase-2.md — On Reopen

If later contradiction or human request forces reopening Phase 2:

- add a reopen notice and cleanup plan to the top of `docs/lifecycle/state/phase-2.md`,
- summarize what invalidated scope,
- summarize what downstream cleanup is required,
- mark affected later-phase durable docs invalid at the top,
- stop for explicit human approval before continuing.
