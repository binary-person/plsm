# docs/lifecycle/execution/phase-0.md — Phase 0 Execution Protocol

## docs/lifecycle/execution/phase-0.md — Phase Name

Phase 0 — PLSM Onboarding

## docs/lifecycle/execution/phase-0.md — Purpose

Parse an existing repository to build an informed understanding of phases 1–4 before entering the normal lifecycle. Phase 0 replaces blank-slate discovery with targeted, evidence-based questions.

Phase 0 is not a normal lifecycle phase. It has no durable output of its own — its output is a correctly populated set of phase 1–4 artifacts. It is entered only once, at the start of PLSM adoption on an existing project.

## docs/lifecycle/execution/phase-0.md — Preconditions

- `current_phase` must be `0`.
- The human must have said `Initiate Phase 0 PLSM onboarding`.

## docs/lifecycle/execution/phase-0.md — Scrap Paper

Use `docs/lifecycle/scrap-paper.md` to record findings across passes. Split into `docs/lifecycle/scrap-paper-*.md` if the file gets heavy. These files are working memory only — they must be deleted after `Approve Phase 0 overview analysis` and must not be referenced by any other lifecycle file.

Scrap paper must always contain two clearly separated sections:

**Section 1 — Parsing findings** (updated each pass):
- files read and key findings from each,
- tentative understanding of the user, problem, and context,
- tentative scope and non-goals inferred from the codebase,
- durable state found — persistence, interfaces, behavioral expectations,
- architecture shape inferred,
- release and DevOps signals found,
- versioning signals found,
- gaps — things that cannot be inferred and must be answered by the human,
- contradictions — things that appear inconsistent and need resolution.

**Section 2 — Overview revision state** (added after parsing is complete):
- current draft of the big picture overview,
- list of corrections the human has already made in this revision loop,
- open items the human has not yet responded to,
- whether `Approve Phase 0 overview analysis` has been received.

A fresh AI resuming Phase 0 must read both sections to understand where the work stands. If Section 2 exists, parsing is complete and the AI is in the overview revision loop. It should present the current draft overview and any outstanding open items rather than re-parsing the repository.

## docs/lifecycle/execution/phase-0.md — Pass Structure

Phase 0 runs across multiple passes until the full repository has been read.

Each pass must:
1. Read a bounded portion of the repository — source files, existing docs, config files, entry points, test files.
2. Update `docs/lifecycle/scrap-paper.md` with findings.
3. Note any new gaps or contradictions found.
4. Stop and confirm with the human if a fundamental ambiguity is found that would change the direction of parsing.

Do not ask routine questions during parsing passes. Save all questions for the overview presentation after parsing is complete.

## docs/lifecycle/execution/phase-0.md — What To Read

Read in this order, stopping to record findings after each category:

1. Entry points — main files, binaries, CLI definitions.
2. Existing docs — README, any architecture or design notes, existing user documentation.
3. Config files — schema files, config formats, environment variable definitions.
4. Source structure — directory layout, module boundaries, key interfaces.
5. Tests — what behaviors are tested, what contracts they imply.
6. Build and release files — Makefile, CI config, release scripts, package manifests.

## docs/lifecycle/execution/phase-0.md — Overview Presentation

After the repository has been fully parsed, present a big picture overview to the human covering:

- **User and problem** — who this is for and what it solves, as understood from the code and docs.
- **Scope** — what the project appears to do and not do.
- **Durable state** — all durable state found: persistence, interfaces (CLI args, API shapes, config formats, env vars), behavioral expectations.
- **Architecture** — the shape of the system as implemented.
- **Release and DevOps** — how the project appears to be built and released.
- **Versioning** — any versioning signals found.
- **Gaps** — things that could not be inferred and must be answered by the human.
- **Contradictions** — things that appear inconsistent across the codebase and need resolution.

Present this as a coherent narrative, not a file-by-file report. The goal is for the human to be able to read it and say "yes, that's what this project is" — or correct what is wrong.

## docs/lifecycle/execution/phase-0.md — Overview Revision

After the overview is presented, the human may correct, adjust, or expand it. The AI must incorporate those corrections before proceeding.

This revision loop continues until the human is satisfied. Do not advance until the human gives the exact approval phrase.

The overview revision is about the big picture only. Fine detail — specific field definitions, exact migration rules, precise scope boundaries — belongs in phases 1–4, not here.

When the overview is ready, ask:

> To approve the overview and proceed to writing phase artifacts, reply exactly: `Approve Phase 0 overview analysis`

## docs/lifecycle/execution/phase-0.md — Writing Phase Artifacts

On exact approval:

1. Write all four phase artifacts at once:
   - `docs/lifecycle/state/phase-1.md` — user, problem, context, success criteria as understood. Mark gaps explicitly.
   - `docs/lifecycle/state/phase-2.md` — scope, non-goals, deferred items as understood. Mark gaps explicitly.
   - `docs/lifecycle/state/phase-3.md` — full durable state inventory across all categories. Mark gaps explicitly.
   - `docs/lifecycle/state/phase-4.md` — architecture, release contract, DevOps, versioning as understood. Mark gaps explicitly.
   - `docs/product/problem.md` — populated from phase 1 findings.
   - `docs/product/scope.md` — populated from phase 2 findings.
   - `docs/maintainers/state-contract.md` — populated from phase 3 findings.
   - `docs/maintainers/architecture.md` — populated from phase 4 findings.
   - `docs/maintainers/release-contract.md` — populated from phase 4 findings.
   - `docs/maintainers/devops.md` — populated from phase 4 findings.
   - `docs/maintainers/versioning-boundaries.md` — populated from phase 4 findings.
   - `docs/maintainers/human-actions.md` — populate with any human-only actions discoverable from the codebase (Makefile release targets, CI publish steps requiring manual triggering, secret setup visible in config). Mark anything uncertain as a gap for the human to verify during Phase 4.

2. Gaps must be written as explicit placeholder sections, not omitted. Each gap should state what is missing and why it could not be inferred.

3. Existing project docs that clearly define something contract-relevant should be linked rather than duplicated. If an existing doc is the authoritative source for a piece of durable state, reference it from the state contract rather than copying its content. If the existing doc is scattered or mixed with non-contract content, note the gap and let the human decide how to restructure it during the relevant phase.

4. Delete all scrap-paper files after artifacts are written.

5. Update `docs/lifecycle/state.yml`:
   - `current_phase: 1`
   - `current_state: "ready_to_execute"`
   - `blocking_now: false`
   - `blocking_reason: ""`
   - `stop_reason: "Phase 0 complete. Phase 1–4 artifacts written with gaps noted. Ready to begin Phase 1."`
   - `next_required_human_action: "Review phase 1 artifact and answer any gaps noted. Then continue to begin Phase 1."`
   - `current_execution_protocol: "docs/lifecycle/execution/phase-1.md"`
   - `current_phase_artifact: "docs/lifecycle/state/phase-1.md"`
   - `linked_artifacts: ["docs/product/problem.md"]`
   - `project_anchor_docs: ["docs/product/problem.md", "docs/product/scope.md"]`

6. Inform the human that all four phase artifacts have been written, scrap-paper has been deleted, and the project is now at Phase 1 with pre-populated context. Gaps are noted in each artifact and will be addressed as each phase is worked through in order.

## docs/lifecycle/execution/phase-0.md — Valid Commands In This Phase

- `Initiate Phase 0 PLSM onboarding` — begins Phase 0.
- `Approve Phase 0 overview analysis` — triggers artifact writing and advances to Phase 1.

No other lifecycle commands are valid during Phase 0.
