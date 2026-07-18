# Decision Framework

*The checklist you run before committing to any major design or technical decision, plus the
format for recording it. A feature that can't answer these questions probably isn't ready.
Six months from now, when you wonder "why did I do this?", the log is the answer — and it
gives AI agents the* reasoning *behind the architecture, not just the architecture.*

---

## The pre-decision checklist

Before committing to a major decision, answer:

1. **Why are we doing this?** What problem or opportunity, in one sentence.
2. **Which design pillar does this support?** If none, why does it exist?
3. **Can this be prototyped first?** What's the cheapest version that tests the assumption?
4. **What existing systems change?** Especially cross-cutting concerns (net, save, input).
5. **Is there a simpler solution?** Name the simplest option you're rejecting, and why.
6. **Will this increase maintenance cost?** New dependencies, new abstractions, new surface area.
7. **How will we know if it worked?** The measurable signal, defined *before* you build.
8. **Can we remove it later?** Is this reversible, or are we locking the design in?

If a decision can't answer 1, 2, and 7, it's not a decision yet — it's a hunch.

## Decision Log format

Every significant decision gets an entry. Keep them append-only; when a decision is
reversed, add a *new* entry that supersedes the old one (don't rewrite history).

```
## Decision #N — <short title>
Date: YYYY-MM-DD
Status: Proposed | Accepted | Superseded by #M

Question:
  <the decision to be made>

Alternatives:
  - <option A>
  - <option B>
  - <option C>

Chosen:
  <the option>

Reason:
  <why — tied to a pillar or a lesson>

Tradeoffs:
  <what we give up>

Reversible?:
  <yes/no; if no, what locks it in>
```

---

## Decision Log

## Decision #1 — RedShiftOS v0.1 scope: foundation only, no stub files

Date: 2026-07-17
Status: Accepted

Question:
  What goes in the first version of RedShiftOS — the full multi-folder document tree, or a
  minimal foundation?

Alternatives:
  - Full tree now: ~30 documents across FOUNDATION / DESIGN / ENGINEERING / AI / PRODUCTION,
    created as stubs to be filled in later.
  - Foundation only: README + four cornerstone documents, no empty folders or stub files.

Chosen:
  Foundation only — README, Development Manifesto, Lessons Learned, Decision Framework,
  Feature Lifecycle.

Reason:
  The OS exists to prevent organic sprawl (the core Cart Clash lesson). Creating 30 empty
  stubs *is* organic sprawl — it recreates the exact anti-pattern the manifesto warns
  against. A document should exist only when it holds real content. Simplicity beats
  cleverness (Manifesto §7).

Tradeoffs:
  The eventual structure (DESIGN, ENGINEERING, AI, PRODUCTION, TEMPLATES, GAME_TEMPLATE) is
  not visible up front, so the shape has to be discovered as content earns its place.

Reversible?:
  Yes — folders and documents are added at any time when they have real content to hold.

## Decision #2 — Agent wiring: how games consume the OS

Date: 2026-07-17
Status: Accepted

Question:
  How do agents get pointed at RedShiftOS, and where does the wiring live?

Alternatives:
  - Hand-roll each game's rules from scratch (the ai-builder-playbook era — it drifted and
    the current version got "botched").
  - A canonical `AGENTS.md` in the OS + a copy-in `GAME_TEMPLATE/` every game repo uses.

Chosen:
  Root `AGENTS.md` (canonical, read-first) + thin `CLAUDE.md` pointer + `GAME_TEMPLATE/`
  (`AGENTS.md` + `PROJECT.md` + `README.md`) that a new game copies. RedShiftOS rides beside
  each game as a git submodule or sibling checkout. Session load order = game `AGENTS.md` →
  OS `FOUNDATION` → OS `PROCESS` → game `PROJECT.md` → tasks / latest handoff.

Reason:
  Mirrors what already works in cart-rave (`AGENTS.md` canonical, `CLAUDE.md` pointer) and
  Wyatt's own ai-builder-playbook starter-template. Rules live in the repo and every session
  reads them first; the OS is the shared context that stops per-game drift.

Tradeoffs:
  Each game must keep RedShiftOS in sync (submodule pin vs. sibling checkout). `GAME_TEMPLATE`
  is real, usable content, so it earns its folder under Decision #1's rule.

Reversible?:
  Yes — the wiring is copy-in; templates evolve as real games reveal what they need.

## Decision #3 — Fold the best of ai-builder-playbook into the OS

Date: 2026-07-17
Status: Accepted

Question:
  Wyatt's earlier `ai-builder-playbook` repo (main / v5 / v6 snapshots) holds a lot of
  hard-won AI-workflow guidance, but it drifted and the current version is "botched." What
  gets imported into RedShiftOS?

Alternatives:
  - Leave it in the playbook repo and reference it.
  - Fold the transferable nuggets into RedShiftOS, routed to the right docs.

Chosen:
  Fold in all four reviewed bundles, routed by fit (not dumped in one place):
  - **Verification** → Feature-Lifecycle (dual-gate + proof ladder) and Lesson L-14
    (visible-bug ground-truth).
  - **Agent working-loop** → AGENTS.md (the loop + writer-reviewer + single-top-task), Lesson
    L-11 (checkpoint commits), `PROCESS/Session-Handoffs.md`, `GAME_TEMPLATE/Tasks.md`.
  - **AI layer** → new `AI/` folder: `Model-Routing.md` (+ velocity debt),
    `Triage-and-Recovery.md`, `Prompt-Library.md`.
  - **Re-imported depth v6 dropped** → `REFERENCE/Assets-and-Provenance.md` + Feature-Lifecycle
    (say-the-feeling, kill-a-prototype).

Reason:
  It's Wyatt's own proven material and the playbook was decaying; RedShiftOS is the durable
  home. Routing by fit (vs. one dumping-ground doc) keeps each doc single-responsibility and
  the always-read rules files dense.

Tradeoffs:
  Some playbook depth was deliberately left behind for now (full stack-selection tables, the
  complete prompt library, publishing / OWASP-LLM safety) — captured in session notes, to
  import if a game actually needs it.

Reversible?:
  Yes — docs can be trimmed or merged as real games show what's actually used.
