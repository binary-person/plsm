# docs/lifecycle/humans/03-how-ai-and-human-work-together.md — How The AI And Human Work Together

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — The Relationship

The lifecycle assumes one human is playing both:

- product owner,
- and lead engineer.

The AI is a disciplined guide and operator.

It should not silently make product decisions that belong to the human.

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — Decision Boundaries

```mermaid
flowchart LR
    H[Human] -->|owns| D1[problem]
    H -->|owns| D2[scope]
    H -->|owns| D3[tradeoffs]
    H -->|owns| D4[approval]

    A[AI] -->|owns| O1[next-step guidance]
    A -->|owns| O2[question batching]
    A -->|owns| O3[artifact updates]
    A -->|owns| O4[consistency checks]
    A -->|owns| O5[reopen/escalation routing]
```

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — What The AI Does In One Pass

```mermaid
sequenceDiagram
    participant Human
    participant AI
    participant State as state.yml
    participant Artifacts as Phase Artifact + Linked Docs

    Human->>AI: continue the current state of the project
    AI->>State: read
    AI->>Artifacts: read current phase artifact + linked docs
    AI->>AI: sanity check
    AI->>Human: ask one batch OR ask approval OR ask for human action
    AI->>Artifacts: update current truth if useful progress happened
    AI->>State: update runtime cursor
```

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — The Three Main Ways A Pass Stops

### 1. The AI needs answers

The AI asks one complete batch of questions and stops.

### 2. The AI needs approval

The AI says exactly what phrase counts as approval, for example:

- `Approve Phase 4`

It does **not** guess approval from tone.

### 3. The human must do something outside AI control

Examples:
- merge a PR,
- configure a repo setting,
- create a secret,
- publish a release,
- verify an external environment change.

The AI must say:
- what to do,
- why,
- how it will check completion,
- and what evidence it will accept.

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — Lifecycle Commands

<!-- source of truth: docs/lifecycle/entrypoint.md -->

The human can explicitly control the lifecycle with commands. The authoritative list lives in `docs/lifecycle/entrypoint.md`. The commands are:

- `Initiate Phase 0 PLSM onboarding` — begins Phase 0 for an existing project.
- `Approve Phase 0 overview analysis` — approves the Phase 0 big picture and triggers artifact writing.
- `Approve Phase X` — approves the current phase and advances to the next.
- `Approve Phase 5 Plan` — confirms the initial implementation plan before coding begins.
- `Reopen Phase X` — reopens an earlier phase due to contradiction or change of intent.
- `Run Phase 8 audit` — triggers a continuity audit from any phase.

These are not casual phrases. They are treated as real lifecycle commands.

## docs/lifecycle/humans/03-how-ai-and-human-work-together.md — Why Exact Phrases Matter

The exact-phrase rule keeps the system simple:

- no guessing,
- no fuzzy approval,
- no accidental rewinds,
- no silent advancement.

```mermaid
flowchart TD
    A[AI asks for exact phrase] --> B{Human response}
    B -->|exact phrase| C[State changes]
    B -->|discussion only| D[No lifecycle transition]
```
