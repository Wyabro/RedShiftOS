# AGENTS.md — <GAME NAME>

**Read this first, every session.** This game is built with **RedShiftOS**, Red Shift Studios'
Game Development Operating System. `CLAUDE.md` points here; point other agents (Cursor, Copilot,
Windsurf) here too.

---

## Load context in this order

1. **This file.**
2. **RedShiftOS** (checked out at `./RedShiftOS` as a submodule, or a sibling directory):
   - `RedShiftOS/AGENTS.md` — the task map: what to load for the task in front of you
   - `RedShiftOS/FOUNDATION/Development-Manifesto.md` — the 10 build rules (binding)
   - then pull the rest of `FOUNDATION/` (Lessons, Anti-Patterns, Decision-Framework,
     Feature-Lifecycle, Philosophy) *by task*, per that map — don't boot-load it all
   - Godot prove: also `RedShiftOS/LIFECYCLE/3-ENGINEERING/Godot.md` + `RedShiftOS/AI/Agent-Loops.md`
3. **`PROJECT.md`** — this game's contract (concept, stack, first shippable version, cut list).
4. **`STATUS.md` / `Tasks.md` / latest handoff** — what's in flight right now.

The RedShiftOS **Manifesto is binding** and overrides your defaults. **Lessons Learned are
binding when the task pulls them** (see `RedShiftOS/AGENTS.md` → "Load by task"). Non-trivial
features run the Feature Lifecycle — pick the task class and run its gates; a dropped gate is a
named choice, not a silent skip. Prove it before you build it.

**Until KEEP:** one question, greybox, stock Godot 4.7, two players if 1.0 is co-op. No
production architecture. You may report TECH PASS or AGENT REVIEW. You may not write HUMAN PASS.

---

## This game (fill from PROJECT.md)

- **Concept:** <one sentence>
- **Stack:** Godot 4.7.x / <language> / <hosting>
- **Players / authority:** <1 or 2 local prove> / <host-auth or n/a>
- **Current evidence state:** <from PROJECT.md>
- **Current milestone / "done":** <what KEEP or this card means>

## Non-negotiables

- Work only the single top task in `Tasks.md` (the human sets priority); explain the plan
  before broad edits.
- Targeted edits only — no whole-file rewrites or refactors unless the plan says so; don't
  touch unrelated files.
- No new dependencies without written justification + approval. No GDExtension in prove.
- "Done" = verified in the build you ship, not "it compiles." Review your own diff.
- Commit surgically (name files; never `git add -A`).
- Ask before changing scope. If context degrades, write a handoff and restart.
- Add game-specific rules and "patterns that already burned us" to `PROJECT.md`, and link
  them here.
- Spike prototypes live in `prototypes/<idea>/` or a throwaway branch. KEEP = rewrite, not
  promote the spike.
