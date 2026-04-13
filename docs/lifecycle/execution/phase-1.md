# docs/lifecycle/execution/phase-1.md — Phase 1 Execution Protocol

## docs/lifecycle/execution/phase-1.md — Phase Name

Phase 1 — Discovery

## docs/lifecycle/execution/phase-1.md — Purpose

Convert a vague software idea into a clear statement of:

- who the user is,
- what problem matters,
- where and how the software will be used,
- why the project should exist,
- what success looks like,
- and what alternatives already exist.

## docs/lifecycle/execution/phase-1.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-1.md`
- `docs/product/problem.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-1.md — Preconditions

- `current_phase` must be `1`.
- No later phase may execute before this phase is approved.

## docs/lifecycle/execution/phase-1.md — AI Operating Mode

Use:

- interrogate,
- synthesize,
- constrain,
- verify.

## docs/lifecycle/execution/phase-1.md — Question Batch

Ask only if the current artifacts do not already make the answers obvious.

When needed, ask one full batch covering:

- Who is the user?
- What problem is worth solving?
- Why should anyone care?
- What will users already know?
- Where will they use it?
- How will they use it?
- What learning curve is acceptable?
- What are the closest alternatives or competitors?
- How should success be measured?

## docs/lifecycle/execution/phase-1.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-1.md`
- `docs/product/problem.md`

## docs/lifecycle/execution/phase-1.md — What To Write

Write current truth only. Do not preserve stale history.

`docs/product/problem.md` must contain:

- user definition,
- problem definition,
- context of use,
- why this matters,
- success criteria,
- alternatives and substitutes,
- concise assumptions.

`docs/lifecycle/state/phase-1.md` must contain:

- current status,
- resolved discovery summary,
- unresolved questions,
- linked durable artifacts,
- approval section,
- reopen notice section when needed,
- next-step note.

## docs/lifecycle/execution/phase-1.md — Blocking Points

Stop if:

- discovery questions need human answers,
- the human rejects the synthesized problem framing,
- the human asks to restart discovery,
- lifecycle corruption is detected.

## docs/lifecycle/execution/phase-1.md — Approval Gate

When the discovery artifact is coherent and complete, ask directly:

> To approve this phase, reply exactly: `Approve Phase 1`

Do not advance without that exact phrase.

## docs/lifecycle/execution/phase-1.md — On Approval

On exact approval:

- mark Phase 1 approved in `docs/lifecycle/state/phase-1.md`,
- set `current_phase: 2`,
- set `current_state: "ready_to_execute"`,
- set `blocking_now: false`,
- set `blocking_reason: ""`,
- set `stop_reason: "Phase 1 approved. Advanced to Phase 2."`,
- set `next_required_human_action: "Answer Phase 2 scope questions if the AI asks them."`,
- set `expected_human_reply: null`,
- set `current_execution_protocol: "docs/lifecycle/execution/phase-2.md"`,
- set `current_phase_artifact: "docs/lifecycle/state/phase-2.md"`,
- set `linked_artifacts` to `docs/product/scope.md`.

## docs/lifecycle/execution/phase-1.md — On Reopen

If later contradiction or human request forces reopening Phase 1:

- add a reopen notice and cleanup plan to the top of `docs/lifecycle/state/phase-1.md`,
- summarize what invalidated discovery,
- summarize what downstream cleanup is required,
- mark affected later-phase durable docs invalid at the top,
- stop for explicit human approval before continuing.
