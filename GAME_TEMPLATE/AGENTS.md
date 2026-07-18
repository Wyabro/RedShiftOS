# AGENTS.md — <GAME NAME>

**Read this first, every session.** This game is built with **RedShiftOS**, RedShift's Game
Development Operating System. `CLAUDE.md` points here; point other agents (Cursor, Copilot,
Windsurf) here too.

---

## Load context in this order

1. **This file.**
2. **RedShiftOS** (checked out at `./RedShiftOS` as a submodule, or a sibling directory):
   - `RedShiftOS/FOUNDATION/Development-Manifesto.md` — the 10 build rules
   - `RedShiftOS/FOUNDATION/Lessons-Learned.md` — mistakes already made, now rules
   - `RedShiftOS/FOUNDATION/Decision-Framework.md` — how decisions get made + the log
   - `RedShiftOS/PROCESS/Feature-Lifecycle.md` — the pipeline every feature follows
3. **`PROJECT.md`** — this game's contract (concept, stack, first shippable version, cut list).
4. **`STATUS.md` / `Tasks.md` / latest handoff** — what's in flight right now.

The RedShiftOS **Manifesto and Lessons Learned are binding** and override your defaults.
Every feature runs through the Feature Lifecycle — no skipping. Prove it before you build it.

---

## This game (fill from PROJECT.md)

- **Concept:** <one sentence>
- **Stack:** <engine / language / hosting>
- **Current milestone / "done":** <what shipping this milestone means>

## Non-negotiables

- Work only the single top task in `Tasks.md` (the human sets priority); explain the plan
  before broad edits.
- Targeted edits only — no whole-file rewrites or refactors unless the plan says so; don't
  touch unrelated files.
- No new dependencies without written justification + approval.
- "Done" = verified in the build you ship, not "it compiles." Review your own diff.
- Commit surgically (name files; never `git add -A`).
- Ask before changing scope. If context degrades, write a handoff and restart.
- Add game-specific rules and "patterns that already burned us" to `PROJECT.md`, and link
  them here.
