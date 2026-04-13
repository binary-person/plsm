# docs/lifecycle/entrypoint.md — Lifecycle Entrypoint

## docs/lifecycle/entrypoint.md — Purpose

This file is the single operational entrypoint for a fresh AI agent with no prior chat context.

The AI must use this file plus `docs/lifecycle/state.yml` to determine:

- where the project currently is,
- why work previously stopped,
- whether work is blocked now,
- what file to read next,
- what artifacts are authoritative,
- and what the next explicit blocking point is.

## docs/lifecycle/entrypoint.md — Source Of Truth Order

Read and trust files in this order:

1. `docs/lifecycle/entrypoint.md`
2. `docs/lifecycle/state.yml`
3. `docs/lifecycle/execution/...` for the current phase
4. `docs/lifecycle/state/phase-*.md` for the current phase
5. files listed in `linked_artifacts`
6. files listed in `project_anchor_docs`
7. `docs/lifecycle/humans/...` for conceptual reference only

## docs/lifecycle/entrypoint.md — Hard Invariants

1. Always read this file first.
2. Always read `docs/lifecycle/state.yml` second.
3. Always perform a startup sanity check before doing any other work.
4. Always read the current phase execution protocol named in `state.yml`.
5. Always read the current phase artifact named in `state.yml`.
6. Always read all linked durable artifacts named in `state.yml`.
7. Always read all files under `project_anchor_docs` if that field is not `null`.
8. Execute exactly one bounded pass.
9. Stop at the next declared blocking point, completion point, explicit pause point, or lifecycle corruption.
10. After every bounded pass that produced useful lifecycle progress, update:
   - the current phase artifact,
   - any linked durable artifacts that the phase owns,
   - and `docs/lifecycle/state.yml`.
11. Never mark a phase complete without the exact explicit approval phrase requested by the protocol.
12. Never silently skip a required gate.
13. Never silently carry unresolved questions forward as assumptions. They must stay explicitly listed until answered or discarded.
14. Never let a sub-agent read or update `docs/lifecycle/*` unless a protocol explicitly requires it.
15. Never let a sub-agent update phase artifacts or `docs/lifecycle/state.yml`.
16. If a contradiction invalidates a prior phase, move the project back immediately to the earliest invalid phase, write a short reopen notice and cleanup plan at the top of that phase artifact, mark affected later-phase durable docs invalid at the top, update `state.yml`, and stop for explicit human approval before continuing.
17. If lifecycle corruption is detected, stop immediately, declare it clearly, and set `current_state: lifecycle_corruption` only if `docs/lifecycle/state.yml` is still writable and trustworthy.

## docs/lifecycle/entrypoint.md — Valid Runtime States

Allowed `current_state` values are exactly:

- `ready_to_execute`
- `waiting_for_human_answers`
- `waiting_for_human_approval`
- `waiting_for_human_action`
- `reopened_due_to_contradiction`
- `reopened_by_human_request`
- `paused_by_ai`
- `phase_complete`
- `lifecycle_corruption`

## docs/lifecycle/entrypoint.md — Startup Sanity Check

On every fresh run, verify all of the following:

- `docs/lifecycle/state.yml` exists and is parseable YAML.
- `current_phase` is a valid phase number: 0, 1, 2, 3, 4, 5, 6, 7, or 8.
- `current_state` is one of the allowed values above.
- `blocking_now` is boolean.
- `current_execution_protocol` exists.
- `current_phase_artifact` exists.
- every file listed under `linked_artifacts` exists.
- every file listed under `project_anchor_docs` exists, unless `project_anchor_docs: null`.
- if `current_state` says the phase is complete, the current phase artifact contains an explicit approval section.
- the execution file, current phase artifact, linked artifacts, and project anchor docs do not contradict `state.yml` in a non-recoverable way.

If any check fails, treat it as lifecycle corruption.

## docs/lifecycle/entrypoint.md — Phase 0 Detection

If the startup sanity check passes and all of the following are true:

- `current_phase` is `1`,
- `current_state` is `ready_to_execute`,
- `current_phase_artifact` shows no prior work has been done (status is `not_started`, no resolved truth recorded),

then look for clear signals of an existing project. Strong signals are:

- source code files (any `.go`, `.py`, `.ts`, `.rs`, `.rb`, `.java`, `.c`, `.cpp`, `.swift`, or similar),
- a build or package manifest (`Makefile`, `package.json`, `go.mod`, `Cargo.toml`, `pyproject.toml`, or similar),
- test files,
- a CI config file (`.github/workflows/`, `.gitlab-ci.yml`, or similar),
- a README that contains installation instructions, usage examples, or CLI reference.

If strong signals are clearly present, suggest Phase 0 before proceeding:

> This appears to be an existing project. If you would like PLSM to parse the repository and build an informed understanding before beginning Phase 1, reply exactly: `Initiate Phase 0 PLSM onboarding`
>
> Otherwise, reply `Continue` to begin Phase 1 from scratch.

If the signals are ambiguous — for example, only a README is present with no code — ask the human directly before assuming anything:

> I can see some files but I am not sure whether this is an existing project or a new one just getting started. Could you clarify? If this is an existing project with code already written, I can parse it first before beginning Phase 1.

Do not guess. Asking costs one message. Parsing a new project as if it were existing wastes many.

If the human says `Initiate Phase 0 PLSM onboarding`, set:
- `current_phase: 0`
- `current_state: "ready_to_execute"`
- `current_execution_protocol: "docs/lifecycle/execution/phase-0.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-1.md"`
- `linked_artifacts: []`
- `project_anchor_docs: null`

Then route to `docs/lifecycle/execution/phase-0.md`.

## docs/lifecycle/entrypoint.md — Lifecycle Corruption

Lifecycle corruption includes, at minimum:

- missing or unreadable `docs/lifecycle/state.yml`,
- malformed YAML in `docs/lifecycle/state.yml`,
- invalid phase number,
- missing execution protocol,
- missing current phase artifact,
- missing linked durable artifact,
- missing project anchor doc when the field is not `null`,
- `state.yml` claiming a phase is complete when the phase artifact does not contain explicit approval,
- a current artifact pointing to downstream durable docs that are expected to exist but do not,
- non-recoverable contradiction between `state.yml` and the current artifact set.

When corruption is detected:

1. Do not continue normal lifecycle execution.
2. Clearly report the corruption.
3. Do not invent missing information.
4. Stop and wait for explicit human correction.
5. If safe, update `docs/lifecycle/state.yml` with:
   - `current_state: lifecycle_corruption`
   - `blocking_now: true`
   - `blocking_reason: <short reason>`
   - `stop_reason: "Lifecycle corruption detected."`

## docs/lifecycle/entrypoint.md — One Pass

One pass means one bounded execution of exactly one lifecycle protocol up to the next declared stop.

A pass must do all of the following in order:

1. Read `docs/lifecycle/state.yml`.
2. Read the current execution protocol.
3. Read the current phase artifact.
4. Read all linked durable artifacts.
5. Read all project anchor docs, unless `project_anchor_docs: null`.
6. Determine the exact current sub-state, blocking condition, and next intended action.
7. If needed, ask one complete batch of clarifying questions.
8. If enough information is present, synthesize or update the owned artifacts for the current phase.
9. If the phase reaches an approval gate, ask directly for approval and state the exact required phrase.
10. If the phase reaches a human-action gate, state exactly what the human must do, how completion will be checked, and what evidence will be accepted.
11. Update `docs/lifecycle/state.yml`.
12. Stop.

Do not execute multiple major lifecycle states in one pass, except that after a phase is explicitly approved the same pass may advance `state.yml` to the next phase and ask the first question of that next phase. If no useful answer is received after that question, do not write new unresolved items for the next phase.

## docs/lifecycle/entrypoint.md — Questioning Rules

Question only when needed to remove ambiguity that blocks current-phase completion.

When questioning is needed:

- ask everything needed in one batch,
- do not ask one question at a time,
- do not ask questions whose answers are already obvious from existing artifacts,
- prefer concrete option-based questions when possible,
- keep questions scoped to the current phase,
- record unresolved questions in the current phase artifact so a fresh AI can resume,
- never silently absorb unresolved questions into phase truth as assumptions,
- delete unresolved questions once they are resolved or explicitly discarded by the human.

## docs/lifecycle/entrypoint.md — Approval Rules

A phase completes only when the human provides the exact approval phrase requested by that phase protocol.

At the end of a phase, the AI must:

1. ask directly for approval,
2. state the exact phrase that counts as approval,
3. refuse to infer approval from enthusiasm, silence, or partial agreement,
4. update `state.yml` to the next phase in the same pass once approval is received.

## docs/lifecycle/entrypoint.md — Human Lifecycle Commands

The human may issue lifecycle control commands at any time.

Supported explicit commands are:

- `Approve Phase X` — approves the current phase and advances to the next.
- `Approve Phase 0 overview analysis` — approves the Phase 0 big picture overview and triggers artifact writing.
- `Approve Phase 5 Plan` — approves the initial implementation plan before implementation begins.
- `Reopen Phase X` — reopens an earlier phase due to contradiction or change of intent.
- `Run Phase 8 audit` — triggers a continuity audit from any phase.
- `Initiate Phase 0 PLSM onboarding` — begins Phase 0 onboarding for an existing project.

When the human appears confused, the AI must restate the exact accepted phrase before treating the input as a lifecycle command.

`Reopen Phase X` is a state-changing lifecycle command. It is not the same as a read-only discussion of an earlier phase.

## docs/lifecycle/entrypoint.md — Human Action Gates

A human-action gate is different from approval.

Approval means the human accepts a phase or output.

Human action means the human must perform a real action outside the AI's current execution, such as:

- repo or platform administration,
- secret or credential setup,
- PR merge,
- release tagging or publishing,
- external system verification,
- environment-specific manual steps,
- policy or organizational approval,
- project-specific operational action.

At a human-action gate, the AI must state:

- exactly what the human must do,
- why the action is needed,
- how completion will be checked,
- what evidence counts as sufficient,
- what happens if the AI cannot verify completion.

## docs/lifecycle/entrypoint.md — Reopen Rules

If a later phase reveals that an earlier phase was wrong or incomplete, or the human explicitly requests a reopen:

1. Move `state.yml` back to the requested or earliest invalid phase.
2. Set `current_state` to either `reopened_due_to_contradiction` or `reopened_by_human_request`.
3. Set `blocking_now: true`.
4. Add a short reopen notice and cleanup plan to the top of that phase artifact containing only:
   - what was discovered,
   - why later work is now invalid or risky,
   - what downstream docs are now invalid,
   - what cleanup is required before continuing.
5. Add temporary invalidation notices to affected later-phase durable docs.
6. Delete the reopen notice after the problem is resolved and cleanup is approved.
7. Stop and wait for explicit human approval before continuing.

**Behavior during a reopened phase:** The AI already has prior decisions recorded in existing phase artifacts. Do not re-run the phase from scratch. Instead, check existing decisions for consistency with what triggered the reopen, and only ask about gaps or things that are genuinely affected. Prior decisions that are still valid should be carried forward without re-asking.

## docs/lifecycle/entrypoint.md — Phase Routing

Use `current_phase` in `docs/lifecycle/state.yml` to route execution:

- 0 → `docs/lifecycle/execution/phase-0.md`
- 1 → `docs/lifecycle/execution/phase-1.md`
- 2 → `docs/lifecycle/execution/phase-2.md`
- 3 → `docs/lifecycle/execution/phase-3.md`
- 4 → `docs/lifecycle/execution/phase-4.md`
- 5 → `docs/lifecycle/execution/phase-5/entrypoint.md`
- 6 → `docs/lifecycle/execution/phase-6.md`
- 7 → `docs/lifecycle/execution/phase-7.md`
- 8 → `docs/lifecycle/execution/phase-8.md`

## docs/lifecycle/entrypoint.md — Callable Phase 8

Phase 8 is a real phase and also callable.

It may be entered:

- through normal lifecycle progression,
- explicitly from Phase 6,
- explicitly from Phase 7,
- or when the human says `Run Phase 8 audit` from any phase.

When the human says `Run Phase 8 audit` from any phase, set:
- `return_to_phase: [current_phase value before this command]`
- `current_phase: 8`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `stop_reason: "Phase 8 audit called from Phase [X]."`
- `next_required_human_action: "Continue the Phase 8 continuity audit."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-8.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-8.md"`
- `linked_artifacts: ["docs/maintainers/continuity.md"]`

Phase 6 and Phase 7 also have their own explicit call blocks which mirror these fields. On completion, Phase 8 returns to `return_to_phase` and restores that phase's state.

When Phase 8 is called rather than entered normally, complete Phase 8 and return to `return_to_phase` unless Phase 8 reveals contradiction requiring an earlier phase to reopen.

## docs/lifecycle/entrypoint.md — Ownership Model

The top-level AI owns lifecycle orchestration.

Sub-agents do not read `docs/lifecycle/*` by default.

Instead, the top-level AI should give sub-agents a distilled task brief containing only:

- task,
- relevant files,
- constraints,
- forbidden changes,
- acceptance checks,
- escalation triggers.

Sub-agents must never update phase artifacts or `docs/lifecycle/state.yml`. They report back to the parent AI.

## docs/lifecycle/entrypoint.md — End-Of-Pass Output

At the end of every pass, the AI must leave the project in a resumable state such that a fresh AI can read:

- `docs/lifecycle/entrypoint.md`
- `docs/lifecycle/state.yml`
- the current execution protocol
- the current phase artifact
- linked durable artifacts
- project anchor docs

and immediately know:

- where the project is,
- why it stopped,
- whether it is blocked now,
- what the next required human response or action is,
- and what to do next.
