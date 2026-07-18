# Model Routing

*Route work to the right model. Don't default to the strongest one for everything, and don't
grind a weak one for hours. The goal is high-signal output per dollar and per minute — and
the biggest hidden cost isn't the model, it's velocity debt (below).*

---

## Route by task

| Use a **frontier / reasoning** model for | Use a **fast / utility / local** model for |
|---|---|
| Architecture & schema design | Boilerplate & scaffolding |
| Security, concurrency, netcode review | Simple tests, styling, naming |
| Planning a large feature | Docs, comments, small refactors |
| Subtle-logic / heisenbug debugging | Routine implementation from a clear plan |
| Risky refactors (+ human approval) | Mechanical edits with an obvious shape |

**Escalate on failure.** If a cheaper model stalls or loops, move up a tier — don't fight it
for an hour. If the frontier model stalls too, that's a signal the *task* is
under-specified, not that you need more attempts (see [Triage & Recovery](./Triage-and-Recovery.md)).

This maps onto the project's real multi-model setup (`/council` — Gemini, Codex, Qwen, GLM,
Kimi): use the council for independent review/perspective, route implementation by the table.

## Context discipline

- Feed only the files the task needs. More context is not better context — it dilutes signal.
- **Restart degraded conversations** with a fresh window + the latest handoff, rather than
  pushing through a context that's lost the thread.
- Keep a rough sense of cost-per-session and what spikes it. The usual culprit is a
  degraded-context loop, not the work itself.

## Velocity debt

> **Velocity debt** is the hidden cost of accepting AI output faster than you can comprehend,
> test, and verify it. It *feels* like progress; weeks later it's code you don't understand,
> inconsistent cross-session patterns, and buried bugs.

Pay it down continuously: plan-and-approve before big edits, review every diff, keep slices
small, and periodically ask **"do I actually understand what this does and why it's here?"**
Speed without understanding isn't velocity — it's debt disguised as progress.
