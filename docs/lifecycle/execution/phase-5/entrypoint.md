# docs/lifecycle/execution/phase-5/entrypoint.md — Phase 5 Entrypoint

## docs/lifecycle/execution/phase-5/entrypoint.md — Phase Name

Phase 5 — Implementation

## docs/lifecycle/execution/phase-5/entrypoint.md — Purpose

Run repeated implementation passes without losing the high-level contract.

Phase 5 is the only intentionally multi-pass heavy phase.

## docs/lifecycle/execution/phase-5/entrypoint.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-5.md`
- `docs/todos.md`
- `docs/maintainers/implementation-plan.md`
- `docs/maintainers/architecture.md`
- `docs/maintainers/state-contract.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/human-actions.md`
- every file listed in `project_anchor_docs`, unless it is `null`

Then route into one of the following internal protocols:

- `docs/lifecycle/execution/phase-5/work-pass.md`
- `docs/lifecycle/execution/phase-5/reopen-and-escalate.md`

## docs/lifecycle/execution/phase-5/entrypoint.md — Sub-State Routing

Before routing by `current_state`, check whether Gate 1 has been approved:

- Read `docs/lifecycle/state/phase-5.md`.
- If it does not contain an explicit Gate 1 approval record, Gate 1 has not been confirmed. Go to Gate 1 — Initial Plan Confirmation. Do not route to `work-pass.md`.
- If it contains an explicit Gate 1 approval record, Gate 1 is done. Route by `current_state` as below.

Use `current_state` to route (only after Gate 1 is confirmed approved):

- `ready_to_execute` → `work-pass.md`
- `waiting_for_human_answers` → `work-pass.md`
- `waiting_for_human_approval` → `work-pass.md`
- `waiting_for_human_action` → `work-pass.md`
- `reopened_due_to_contradiction` → `reopen-and-escalate.md`
- `reopened_by_human_request` → `reopen-and-escalate.md`
- `paused_by_ai` → `work-pass.md`

## docs/lifecycle/execution/phase-5/entrypoint.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-5.md`
- `docs/todos.md`
- `docs/maintainers/implementation-plan.md`
- temporary sub-agent brief files matching `docs/lifecycle/state/phase-5-subagent-*.md`

## docs/lifecycle/execution/phase-5/entrypoint.md — Phase 5 Artifact Split

- `docs/maintainers/implementation-plan.md` is the big picture — total progress, milestone structure, how phases 3 and 4 translate into buildable components. It changes gradually and must always reflect the current high-level intent.
- `docs/todos.md` is the current concise work map — what is done, what to ignore right now, what still needs attention, active workstreams broken down from the current milestone. It changes frequently.
- `docs/lifecycle/state/phase-5.md` is the strategic summary for a fresh parent AI.

## docs/lifecycle/execution/phase-5/entrypoint.md — Two Approval Gates

Phase 5 has two approval gates.

### Gate 1 — Initial Plan Confirmation

Entered immediately after Phase 4 is approved, before any implementation begins.

The AI must:
1. Read all Phase 3 and Phase 4 artifacts.
2. Ask the human one batch of questions — only things genuinely unclear:
   - Are there existing user-facing docs (README usage section, website, man page, help strings) that should be treated as authoritative sources of truth, or are they derived from the contract and expected to be updated as implementation produces them?
   - Are there any existing docs heavy enough they should be split before implementation begins?
   - Is this primarily fresh implementation, migration to match the current contract truth, or a mix of both?
   - Any other open questions that would block drafting the plan.
3. Draft an initial implementation plan in `docs/maintainers/implementation-plan.md` containing:
   - how the state contract and architecture translate into buildable components,
   - proposed milestone structure with sequencing rationale,
   - whether this is fresh implementation, migration work, or both,
   - which deliverables are contract artifacts versus derived docs.
4. Present the plan to the human for quick confirmation — this is a living document expected to evolve. The goal is to confirm the AI has the right high-level picture before writing any code.
5. Incorporate any corrections.
6. Ask for confirmation:

> To confirm the implementation plan and begin Phase 5, reply exactly: `Approve Phase 5 Plan`

On exact approval:
- Record Gate 1 as confirmed in `docs/lifecycle/state/phase-5.md` under the Gate 1 section.
- Set `current_state: "ready_to_execute"`
- Set `blocking_now: false`
- Set `blocking_reason: ""`
- Set `stop_reason: "Phase 5 Plan confirmed. Implementation may begin."`
- Set `next_required_human_action: "Review implementation progress or answer implementation questions if the AI asks them."`
- Set `expected_human_reply: null`

Do not begin implementation until this confirmation is received.

If the implementation plan drifts significantly during Phase 5 — major sequencing changes, milestone scope substantially changed — flag it to the human before continuing. Gradual evolution is expected; silent wholesale divergence is not.

### Gate 2 — Milestone Completion Approval

Phase 5 ends when implementation is sufficiently complete for release decisioning and the human explicitly approves moving to Phase 6.

> To approve this phase, reply exactly: `Approve Phase 5`

## docs/lifecycle/execution/phase-5/entrypoint.md — Sub-Agent Rule

The top-level AI may spawn sub-agents for implementation work, but only the parent AI may orchestrate lifecycle state.

Before a sub-agent run:

1. create `docs/lifecycle/state/phase-5-subagent-<uniqueid>.md`,
2. link it from `docs/lifecycle/state/phase-5.md` as a short temporary note,
3. keep the brief limited to:
   - task,
   - relevant files,
   - constraints,
   - forbidden changes,
   - acceptance checks,
   - escalation triggers.

After the sub-agent run finishes:

- fold useful results back into the parent AI's own reasoning,
- update `docs/lifecycle/state/phase-5.md` and `docs/todos.md` if needed,
- delete the temporary brief file,
- delete the temporary note from `docs/lifecycle/state/phase-5.md`.

Sub-agents must never update phase artifacts or `docs/lifecycle/state.yml`.

## docs/lifecycle/execution/phase-5/entrypoint.md — Completion

See the Two Approval Gates section above. Gate 1 (`Approve Phase 5 Plan`) must be received before implementation begins. Gate 2 (`Approve Phase 5`) advances to Phase 6.

On `Approve Phase 5`, set:

- `current_phase: 6`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `stop_reason: "Phase 5 approved. Advanced to Phase 6."`
- `next_required_human_action: "Review release decisioning outputs or answer release questions if the AI asks them."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-6.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-6.md"`
- `linked_artifacts` to:
  - `docs/maintainers/release-decision.md`
  - `docs/maintainers/versioning-boundaries.md`
  - `docs/maintainers/state-contract.md`
  - `docs/maintainers/human-actions.md`.


## docs/lifecycle/execution/phase-5/entrypoint.md — Active Workstream Cap

Phase 5 must not grow into a project-management system. Keep `docs/todos.md` to at most 3 active workstreams at once unless the human explicitly approves more.
