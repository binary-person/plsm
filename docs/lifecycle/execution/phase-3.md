# docs/lifecycle/execution/phase-3.md — Phase 3 Execution Protocol

## docs/lifecycle/execution/phase-3.md — Phase Name

Phase 3 — State Contract

## docs/lifecycle/execution/phase-3.md — Purpose

Define the application's durable-state contract before implementation by making explicit:

- what data exists,
- what is durable versus ephemeral,
- where durable state lives,
- who may mutate it,
- how it evolves,
- and what migration expectations exist.

## docs/lifecycle/execution/phase-3.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-3.md`
- `docs/product/problem.md`
- `docs/product/scope.md`
- `docs/maintainers/state-contract.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-3.md — Preconditions

- `current_phase` must be `3`.
- Phases 1 and 2 must already be approved.

## docs/lifecycle/execution/phase-3.md — AI Operating Mode

Use:

- interrogate,
- synthesize,
- constrain,
- critique,
- verify.

## docs/lifecycle/execution/phase-3.md — Question Batch

Ask only when needed. If needed, ask one full batch covering:

- What data does this system persist? (databases, files written to disk, config formats)
- What is durable versus ephemeral?
- Where does durable state live?
- Who writes it?
- Who reads it?
- What is the source of truth?
- What interfaces does this system expose that users may form expectations around? (CLI argument shapes, API request and response structures, environment variable names, config file keys)
- What behavioral expectations exist that users or dependents rely on? (output formats, side effects, ordering, defaults)
- What concurrency assumptions exist?
- What schema-evolution or migration rules are required?
- For any new durable state being added: is it additive (nothing existing breaks) or breaking (existing users or systems are affected)? Both must be recorded here.
- What rollback or recovery expectations exist?
- Are there existing user-facing docs (README usage section, website, CLI help text, man page) that describe the durable state or interface contracts? If so, should they be treated as the authoritative source of truth, or are they derived from what gets defined here? See below for how to make this judgment.
- Are any existing docs heavy, scattered, or mixed with non-contract content? If so, discuss with the human how to restructure so there is one clear source of truth with no duplication between the contract and derived docs.

## docs/lifecycle/execution/phase-3.md — User Docs: Contract Or Implementation?

When existing user-facing docs describe the system's interfaces or behavior, the human must decide whether those docs are the contract or the implementation:

Ask the human:

> If the user docs and the code disagree on something — for example, the README says a CLI flag is `--output-dir` but the code uses `--out` — which one is wrong?

- If the answer is "the code is wrong, the docs define what it should be" — the docs are the contract. Link them from `docs/maintainers/state-contract.md` as authoritative. Do not duplicate their content.
- If the answer is "the docs are wrong, the code defines what it actually does" — the docs are derived implementation. The contract is defined here in Phase 3, and the docs become a downstream artifact that should reflect the contract.
- If the answer is "it depends" or "both are partially right" — this is a contradiction that must be resolved before Phase 3 can be approved. Record it as an unresolved question and stop for human input.

The key principle: there must be exactly one source of truth for each piece of durable state. User docs and the state contract must never both claim to be authoritative for the same thing.

## docs/lifecycle/execution/phase-3.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-3.md`
- `docs/maintainers/state-contract.md`

## docs/lifecycle/execution/phase-3.md — What To Write

Write current truth only.

`docs/maintainers/state-contract.md` must contain:

- state inventory — covering all durable state categories: persistence (databases, files, config), interfaces (CLI argument shapes, API contracts, environment variables), and behavioral expectations users rely on,
- persistence boundaries,
- source-of-truth statement,
- mutation rules,
- concurrency assumptions,
- schema-evolution rules — including both breaking changes (existing obligations modified, migration required) and additive changes (new obligations created, must be recorded at time of addition),
- migration expectations,
- rollback and recovery expectations.

For any piece of durable state where an existing doc was determined to be authoritative in the question batch, reference that doc as the source of truth rather than duplicating its content. Record the link and note that the external doc is authoritative. Do not copy its content into the state contract.

`docs/lifecycle/state/phase-3.md` must contain:

- current status,
- resolved state-contract summary,
- unresolved questions,
- linked durable artifacts,
- approval section,
- reopen notice section when needed,
- next-step note.

## docs/lifecycle/execution/phase-3.md — Blocking Points

Stop if:

- state questions need human answers,
- state assumptions are not explicit enough,
- migration expectations are missing,
- scope must be reopened to simplify state,
- lifecycle corruption is detected.

## docs/lifecycle/execution/phase-3.md — Approval Gate

When the state contract is coherent and migration-aware, ask directly:

> To approve this phase, reply exactly: `Approve Phase 3`

Do not advance without that exact phrase.

## docs/lifecycle/execution/phase-3.md — On Approval

On exact approval:

- mark Phase 3 approved in `docs/lifecycle/state/phase-3.md`,
- check `return_to_phase` in `docs/lifecycle/state.yml`:
  - if `return_to_phase` is `null` — advance to Phase 4 as normal:
    - set `current_phase: 4`
    - set `current_execution_protocol: "docs/lifecycle/execution/phase-4.md"`
    - set `current_phase_artifact: "docs/lifecycle/state/phase-4.md"`
    - set `linked_artifacts` to `docs/maintainers/architecture.md`, `docs/maintainers/release-contract.md`, `docs/maintainers/devops.md`, `docs/maintainers/versioning-boundaries.md`, `docs/maintainers/human-actions.md`
    - set `next_required_human_action: "Answer Phase 4 architecture, release, DevOps, or versioning questions if the AI asks them."`
  - if `return_to_phase` is `4` — reopened from Phase 7 with Phase 4 also affected, advance to Phase 4:
    - same fields as above
    - set `return_to_phase: 5`
  - if `return_to_phase` is `5` — reopened from Phase 7 for additive recording only, skip Phase 4 and advance to Phase 5:
    - set `current_phase: 5`
    - set `current_execution_protocol: "docs/lifecycle/execution/phase-5/entrypoint.md"`
    - set `current_phase_artifact: "docs/lifecycle/state/phase-5.md"`
    - set `return_to_phase: null`
    - set `linked_artifacts` to `docs/todos.md`, `docs/maintainers/implementation-plan.md`, `docs/maintainers/architecture.md`, `docs/maintainers/state-contract.md`, `docs/maintainers/versioning-boundaries.md`, `docs/maintainers/human-actions.md`
    - set `next_required_human_action: "Review implementation progress or answer implementation questions if the AI asks them."`
- set `current_state: "ready_to_execute"`
- set `blocking_now: false`
- set `blocking_reason: ""`
- set `expected_human_reply: null`
- set `stop_reason` to reflect where the project is advancing.

## docs/lifecycle/execution/phase-3.md — On Reopen

If later contradiction or human request forces reopening Phase 3:

- add a reopen notice and cleanup plan to the top of `docs/lifecycle/state/phase-3.md`,
- summarize what invalidated the state contract,
- summarize what downstream cleanup is required,
- mark affected later-phase durable docs invalid at the top,
- stop for explicit human approval before continuing.
