# docs/lifecycle/humans/07-working-with-ai.md — Working With An AI

## docs/lifecycle/humans/07-working-with-ai.md — Choosing Your AI Environment

Different AI tools handle files differently. This matters because PLSM depends on its files being readable and writable across the full lifecycle.

```
┌─────────────────────────┬────────────────────────────┬──────────────────────────────────┐
│ Environment             │ File persistence            │ Recommended workflow             │
├─────────────────────────┼────────────────────────────┼──────────────────────────────────┤
│ Real filesystem         │ Durable across              │ Point AI to entrypoint.md.       │
│ (Claude Code, Codex,    │ conversations               │ No zip management needed.        │
│ Cursor, Windsurf, etc.) │                             │                                  │
├─────────────────────────┼────────────────────────────┼──────────────────────────────────┤
│ Claude.ai (chat)        │ Persists within one         │ Upload zip at start. Download    │
│                         │ conversation only           │ updated zip at end.              │
├─────────────────────────┼────────────────────────────┼──────────────────────────────────┤
│ ChatGPT Code Interpreter│ Sandboxed — can silently    │ Upload zip at start. Download    │
│                         │ wipe mid-conversation if    │ updated zip at end. Use          │
│                         │ context gets long or the    │ defensive prompt (see below).    │
│                         │ sandbox goes idle           │                                  │
└─────────────────────────┴────────────────────────────┴──────────────────────────────────┘
```

---

## docs/lifecycle/humans/07-working-with-ai.md — Starting Prompts

### For AI tools with a real filesystem (Claude Code, Codex, Cursor, Windsurf, or any tool with direct read/write access)

```
Follow the instructions strictly and faithfully in docs/lifecycle/entrypoint.md.
Before any file changes, always ask for permission unless the permission is
given in the docs/lifecycle/execution/* documents. Do not make assumptions.
If files are suddenly missing, stop immediately and report what happened
concisely. Do not try to recreate the missing files. When doing
rewriting/cleanup, do not use rewriting language such as "... is now ...".
```

### For AI tools with a sandboxed filesystem (Claude.ai, ChatGPT)

Upload your zip first, then:

```
Unzip this and follow the instructions strictly and faithfully in
docs/lifecycle/entrypoint.md. Before any file changes, always ask for
permission unless the permission is given in the docs/lifecycle/execution/*
documents.

We are doing this such that after the conversation, I download the changed
files. In addition, you also update those changed files on your own filesystem
when the instructions tell you to do so.

Delete any stale zips first before creating a new zip, so we always know
definitively the latest zip is the latest zip. Right after zipping, your next
implicit permissible task is to rename the previous zip to before_latest.zip.
For the first run, there should be no stale zip files.

Do not make assumptions. If files are suddenly missing, stop immediately and
report what happened concisely. Do not try to recreate the missing files.
When doing rewriting/cleanup, do not use rewriting language such as "... is now ...".
```

The zip is your durable source of truth. The sequence on every pass is:

1. Delete any zips older than `before_latest.zip` (stale).
2. Do your work.
3. Create a new zip of the current state.
4. Rename the previous zip to `before_latest.zip`.

You always have exactly two zips at most: the fresh one and one backup. On the
very first run there are no zips to manage yet.

The sandboxed filesystem can silently delete itself mid-conversation if the
context gets too long or the sandbox goes idle. Always download the latest
zip at the end of a conversation before closing.

---

## docs/lifecycle/humans/07-working-with-ai.md — Phases 1–4: Do Not Rush These

Phases 1–4 are foundational. Every edge case caught here is an edge case that
does not silently become a bug, a migration problem, or a broken user
expectation in phase 5.

This is where your project is most efficiently and accurately described in a
form a fresh AI can fully understand without reading a single line of code.
The more clear, tight, and anti-drift these phases are, the less work you will
do in the long run.

**After phases 2 and 3:** Open a completely fresh AI with zero context. Paste
in the phase artifacts and ask for improvements or clarifications. Repeat more
than once. Fresh eyes catch drift that the original conversation has normalized.

**Before phase 4 approval:** Open a blank AI and run:

```
Please check phases 1–4 to ensure they are consistent with each other.
Flag anything confusing or any clarification that could send the project
in a different direction.
```

**After the implementation plan is drafted for phase 5:** Open a blank AI and run:

```
Please check phases 3–4 against this implementation plan and confirm
the plan is consistent with the state contract and architecture.
```

---

## docs/lifecycle/humans/07-working-with-ai.md — Getting The AI To Help You Narrow The Product

The goal during phases 1–4 is to push the AI to criticize you accurately. This
is how scope gets precise enough that the AI cannot quietly make product
decisions for you during implementation.

After answering any batch of questions, close with one or more of:

- *Follow up with more questions if you think it could benefit from being more precise. You are welcome to be annoying but accurate.*
- *Is there anything that needs clarification or could benefit from improvement, especially to prevent drift?*
- *There should be no implicit or confusing behavior. If I missed anything, say so.*
- *Please ask clarifying questions that would help narrow the scope. Start with easy ones first.*

If you want the AI to push harder:

- *Let's do a few more tightening passes.*

If the AI gives an example or raises a concern:

- *What do you mean by [X]?*
- *What considerations do I need to be aware of?*

If you are not sure the output is precise enough:

- *I might be missing something. Am I?*

If the AI is being verbose with its concerns:

- *Can you write this in a concise question-by-question format, with suggested answers where the possibilities are obvious?*

---

## docs/lifecycle/humans/07-working-with-ai.md — Phase 4 Rigidity Audit

Before approving phase 4, run a hardening pass. Open the pass by telling the
AI: *"Lock write permission. We're going to do a hardening pass."* This is a
mindset instruction — it signals to the AI to stay in critical/read-only mode
and not update files while you work through these questions. Then ask
all of the following questions. In ChatGPT, put them all in one message edit
rather than sending sequentially — this keeps the full context in one place
so the AI can answer them together. In other tools, send them as one batch:

- How is the implementation plan? Is it clear?
- Is the architecture structured so that tests are easy to create and verify? If not, where are the weak points?
- Which areas are the most likely to drift?
- Which component is the hardest to test or implement?
- Which contract or document is too heavy and worth splitting?

---

## docs/lifecycle/humans/07-working-with-ai.md — Useful Mid-Conversation Prompts

**At the start of each change pass (sandboxed conversations):**

Use this whenever you are about to do any meaningful work in a sandboxed conversation — not just implementation, but any phase where files will be updated.

```
Ok, now delete the stale zips except the latest. Rename the latest zip to
before_latest.zip. Confirm you already have everything in the latest folder.
```

**Implementation passes (sandboxed conversations):**

```
Ok, now start implementation. You are now both the parent AI and the subagent
AI. Continue working on the active workstreams. If there are none, make new
ones. If there is nothing more to work on due to finished or blocked work, say
so. If you are blocked on environment-specific things or assumptions, make a
separate section detailing those concerns. Do not invent or quick-fix
environmental restrictions. If nothing else can be done due to
environmental or network restrictions, stop. Otherwise keep pushing through
the straightforward implementation. Afterwards, save those concerns to
todos.md and give a quick summary at the end of your message.
```

**For plan-driven implementation passes:**

```
For these passes, we have a special docs/PLAN.md that gets us to the milestone
of [X]. You are free to make edits according to phase 5 rules unless you
finish PLAN.md, in which case you stop. Only delete PLAN.md when everything
is done and I give you permission.
```

**Expensive model → cheap model pattern:**

Use a high-reasoning model to write a detailed `PLAN.md` for the next pass,
then hand that plan to a cheaper/faster model to execute it.

```
(Expensive model): Write a detailed implementation plan in PLAN.md for a
fresh AI agent to execute in the next pass.

(Cheaper model): Implement the plan in PLAN.md.
```

---

## docs/lifecycle/humans/07-working-with-ai.md — A General Note On Tone

Treat the AI as a disciplined but fallible thinking partner. It is very good at
detail and very good at asking the questions you forgot to ask yourself. It is
also capable of quietly drifting if there are no guardrails.

Your job is to own the decisions. The AI's job is to surface what you have not
decided yet.
