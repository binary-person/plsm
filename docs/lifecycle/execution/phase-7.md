# docs/lifecycle/execution/phase-7.md — Phase 7 Execution Protocol

## docs/lifecycle/execution/phase-7.md — Phase Name

Phase 7 — Maintenance

## docs/lifecycle/execution/phase-7.md — Purpose

Phase 7 is the standing home for the project after release. It does not complete — it loops. The human brings a change, bug, or insight conversationally. The AI classifies it before any work begins and routes it to the right place.

The post-release loop is: Phase 7 → Phase 5 → Phase 6 → Phase 7.

## docs/lifecycle/execution/phase-7.md — Read Before Acting

Always read:

- `docs/lifecycle/state.yml`
- `docs/lifecycle/state/phase-7.md`
- `docs/maintainers/maintenance.md`
- `docs/maintainers/versioning-boundaries.md`
- `docs/maintainers/state-contract.md`
- `docs/maintainers/human-actions.md`
- every file listed in `project_anchor_docs`, unless it is `null`

## docs/lifecycle/execution/phase-7.md — Preconditions

- `current_phase` must be `7`.

## docs/lifecycle/execution/phase-7.md — Outputs Owned By This Phase

This phase owns and must update:

- `docs/lifecycle/state/phase-7.md`
- `docs/maintainers/maintenance.md`

## docs/lifecycle/execution/phase-7.md — How A Maintenance Signal Enters

Phase 7 is conversational. The human describes what they want in chat — a bug they found, a feature they want, an improvement they noticed. The AI does not wait for a formal document. If the human points to an external doc (a GitHub issue, a bug report, a spec), the AI reads it as context.

When a signal arrives:

1. Restate the change in your own words to confirm understanding.
2. Ask clarifying questions if the change is ambiguous — one focused batch, not one at a time.
3. Once the change is clear, classify it.
4. Record the classification and decision in `docs/maintainers/maintenance.md`.
5. Route using the routing table below.

## docs/lifecycle/execution/phase-7.md — Classification

Consult `docs/maintainers/versioning-boundaries.md` to classify the change:

- **Patch** — fixes something broken, changes no observable behavior, touches no durable state.
- **Minor** — adds new durable state (new CLI flag, new config field, new output field, new behavioral expectation). Nothing existing breaks. The new obligation must be recorded.
- **Major** — changes or removes an existing durable state obligation. Something that currently depends on it would break or be surprised.
- **DevOps / release / versioning change** — affects how the project is built, released, or versioned but not the product itself.
- **Scope change** — expands what the project does or conflicts with a stated non-goal.
- **Problem / user change** — the user definition or problem framing is no longer valid.

When unsure whether something is additive or breaking, ask the human:

> If we ship this without telling existing users, who would be surprised?

## docs/lifecycle/execution/phase-7.md — Routing

**Patch:**
Route directly to Phase 5. No reopen required.

Set:
- `current_phase: 5`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `return_to_phase: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-5/entrypoint.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-5.md"`

**Minor (additive durable state):**
Reopen Phase 3 to record the new obligation. Phase 4 is not affected. After Phase 3 is approved, route to Phase 5.

Set:
- `current_phase: 3`
- `current_state: "reopened_by_human_request"`
- `blocking_now: true`
- `return_to_phase: 5`
- `current_execution_protocol: "docs/lifecycle/execution/phase-3.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-3.md"`

Write a reopen notice in `docs/lifecycle/state/phase-3.md` describing the new obligation being added.

**Major (breaking existing durable state obligation):**
Reopen Phase 3. Determine whether Phase 4 contracts are also affected.

- If Phase 4 is unaffected (the migration strategy fits within existing release and DevOps contracts): set `return_to_phase: 5`.
- If Phase 4 is affected (migration requires release contract changes, new DevOps steps, or versioning boundary updates): set `return_to_phase: 4`.

Set:
- `current_phase: 3`
- `current_state: "reopened_by_human_request"`
- `blocking_now: true`
- `current_execution_protocol: "docs/lifecycle/execution/phase-3.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-3.md"`

Write a reopen notice in `docs/lifecycle/state/phase-3.md` describing what obligation is being changed and what migration is required.

**DevOps / release / versioning change:**
Reopen Phase 4.

Set:
- `current_phase: 4`
- `current_state: "reopened_by_human_request"`
- `blocking_now: true`
- `return_to_phase: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-4.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-4.md"`

**Scope change:**
Reopen Phase 2. The AI already has most prior decisions — check for consistency with existing phase artifacts and only ask about gaps or things the scope change affects.

Set:
- `current_phase: 2`
- `current_state: "reopened_by_human_request"`
- `blocking_now: true`
- `return_to_phase: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-2.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-2.md"`

**Problem / user change:**
Reopen Phase 1. The AI already has most prior decisions — check for consistency with existing phase artifacts and only ask about gaps or things the framing change affects. Normal downstream progression handles 1→2→3→4→5.

Set:
- `current_phase: 1`
- `current_state: "reopened_by_human_request"`
- `blocking_now: true`
- `return_to_phase: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-1.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-1.md"`

## docs/lifecycle/execution/phase-7.md — Valid Commands In This Phase

- `Reopen Phase 1` through `Reopen Phase 4` — human-directed reopen based on classification.
- `Run Phase 8 audit` — callable at any time.
- `Approve Phase 7` — only for an explicit maintenance snapshot before a major handoff, not for routine maintenance.

Not valid from Phase 7:
- `Reopen Phase 5` — Phase 5 is a destination, not reopened from here.
- `Reopen Phase 6` — Phase 6 follows Phase 5.
- `Reopen Phase 7` — already here.
- `Reopen Phase 8` — Phase 8 is callable, not reopenable.

## docs/lifecycle/execution/phase-7.md — Approval Gate

Phase 7 does not require approval on every maintenance item. It is a standing phase. However, if a maintenance decision package is being explicitly accepted before a major handoff, ask:

> To approve this phase snapshot, reply exactly: `Approve Phase 7`

## docs/lifecycle/execution/phase-7.md — Callable Phase 8 Trigger

Phase 7 may call Phase 8 if the human says `Run Phase 8 audit` or if survivability drift is suspected.

When calling Phase 8, set:
- `current_phase: 8`
- `current_state: "ready_to_execute"`
- `blocking_now: false`
- `blocking_reason: ""`
- `return_to_phase: 7`
- `stop_reason: "Phase 8 audit called from Phase 7."`
- `next_required_human_action: "Continue the Phase 8 continuity audit."`
- `expected_human_reply: null`
- `current_execution_protocol: "docs/lifecycle/execution/phase-8.md"`
- `current_phase_artifact: "docs/lifecycle/state/phase-8.md"`
- `linked_artifacts: ["docs/maintainers/continuity.md"]`
