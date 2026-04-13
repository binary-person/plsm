# docs/lifecycle/humans/10-adopting-plsm.md — Adopting PLSM On An Existing Project

## docs/lifecycle/humans/10-adopting-plsm.md — The Problem

PLSM is designed to start at Phase 1 with a clean slate. But most projects do not start that way. There is already a codebase, existing docs, real users, real durable state that was never formally recorded, and scope decisions that were made implicitly rather than written down.

Phase 0 exists to bridge that gap. Instead of asking the human to fill in phases 1–4 from scratch about a project that already exists, Phase 0 has the AI read the repository first, build its understanding, confirm that understanding with the human, then write pre-populated phase 1–4 artifacts with gaps explicitly noted.

---

## docs/lifecycle/humans/10-adopting-plsm.md — How To Start

1. Copy the `docs/` folder into your repository root.
2. Open a conversation with any capable LLM.
3. Use the starting prompt for your AI environment from `docs/lifecycle/humans/07-working-with-ai.md`.

The AI will start the startup sanity check, find the project is at Phase 1 ready-to-execute, notice existing project files, and suggest:

> This appears to be an existing project. If you would like PLSM to parse the repository and build an informed understanding before beginning Phase 1, reply exactly: `Initiate Phase 0 PLSM onboarding`

Reply with that exact phrase to begin.

---

## docs/lifecycle/humans/10-adopting-plsm.md — What Phase 0 Does

### Parsing

The AI reads the repository across multiple passes — entry points, existing docs, config files, source structure, tests, build and release files. It records its findings in `docs/lifecycle/scrap-paper.md`, splitting to `docs/lifecycle/scrap-paper-*.md` if the file gets heavy. These are working memory only and will be deleted when Phase 0 is done.

The AI does not ask questions during parsing. It reads and records.

### Overview presentation

After parsing is complete, the AI presents a big picture overview covering:

- who the project is for and what it solves,
- what it appears to do and not do,
- all durable state found — persistence, interfaces, behavioral expectations,
- the architecture shape as implemented,
- how the project appears to be built and released,
- gaps that could not be inferred,
- contradictions that need resolution.

### Overview revision

You read the overview and correct what is wrong. This is the moment to shape the big picture before it gets written into phase artifacts. The AI incorporates your corrections and presents the revised overview again.

This loop continues until the overview accurately reflects the project. Fine detail belongs in phases 1–4, not here — the overview is about coherence and accuracy at the high level.

When you are satisfied, say exactly: `Approve Phase 0 overview analysis`

### Artifact writing

On approval, the AI writes all phase 1–4 artifacts at once:

- `docs/lifecycle/state/phase-1.md` through `phase-4.md`
- `docs/product/problem.md` and `scope.md`
- `docs/maintainers/state-contract.md`, `architecture.md`, `release-contract.md`, `devops.md`, `versioning-boundaries.md`

Gaps are written as explicit placeholder sections — not omitted. Each gap states what is missing and why it could not be inferred.

Existing docs that clearly define something contract-relevant are linked rather than duplicated. If an existing doc is scattered or mixes contract and non-contract content, Phase 0 notes the gap and leaves the restructuring for the relevant phase.

Scrap-paper files are deleted. The project advances to Phase 1.

---

## docs/lifecycle/humans/10-adopting-plsm.md — What Happens Next

You are now at Phase 1 with pre-populated context. The phase artifacts contain what the AI understood, with gaps marked. You work through phases 1–4 in order, filling gaps and approving each phase normally.

The difference from a blank-slate project is that the AI's questions are targeted — it already knows the broad shape of the project and is asking about specific gaps and ambiguities rather than starting from nothing.

---

## docs/lifecycle/humans/10-adopting-plsm.md — What To Expect

Phase 0 will likely surface durable state that was never formally recorded — CLI flags, config keys, output formats, behavioral expectations that users depend on. This is one of the most valuable things it does. Recording it explicitly in the state contract is what makes future changes safe.

It may also surface contradictions — places where the codebase and existing docs disagree, or where different parts of the code make conflicting assumptions. These are noted as gaps for the human to resolve during the relevant phase.

The overview revision is important. Do not rush it. The AI's parsed understanding is only as good as what the codebase shows — implicit decisions, historical context, and product intent that lives in your head will not show up in the code. The revision step is where you correct the AI's blind spots before they get written into the phase artifacts.
