# AGENTS.md — RedShiftOS

**Canonical rules for working with RedShiftOS. Read this first, every session.** `CLAUDE.md`
points here; other agents (Cursor, Copilot, Windsurf, Cline) should be pointed here too — the
filename varies, the habit doesn't: the rules live in the repo and every session reads them
first.

RedShiftOS is an AI-first **Game Development Operating System** — a docs/process repo, not
code. It carries how RedShift builds games so every agent pulls toward the same north star
instead of optimizing one prompt at a time. It lives *beside* every game.

---

## Context-loading order

Read these, in order, before doing anything else:

1. **This file** (`AGENTS.md`).
2. **`FOUNDATION/Development-Manifesto.md`** — the 10 rules for how games get built here.
3. **`FOUNDATION/Lessons-Learned.md`** — mistakes already made, turned into permanent rules.
4. **`FOUNDATION/Decision-Framework.md`** — how decisions get made, and the append-only log.
5. **`PROCESS/Feature-Lifecycle.md`** — the pipeline every feature follows, idea → merge.

The Manifesto and Lessons Learned are **binding** — they override your defaults. When a
lesson or decision names a file, flag, or command, verify it still exists before relying on
it (these are point-in-time notes, not live state).

---

## If you're building a GAME with this OS

- RedShiftOS is checked out **beside your game repo** — a git submodule at `./RedShiftOS`, or
  a sibling directory. Read its `FOUNDATION/` then `PROCESS/` at session start.
- The game repo has its **own** `AGENTS.md` + thin `CLAUDE.md` that point here — start from
  `GAME_TEMPLATE/` (copy `AGENTS.md` + `PROJECT.md` into the new repo).
- Session load order inside a game: game `AGENTS.md` → RedShiftOS `FOUNDATION` → RedShiftOS
  `PROCESS` → the game's `PROJECT.md` (its contract) → the game's current tasks / status /
  latest handoff.
- Every feature runs through `PROCESS/Feature-Lifecycle.md`. No skipping. Prove it before you
  build it.

## If you're working ON RedShiftOS itself

- **No stub files.** A document exists only when it holds real content (Decision #1). Empty
  scaffolding is the exact sprawl this OS exists to prevent.
- **Lessons-Learned IDs are append-only and stable.** A new lesson gets the next `L-NN`;
  never renumber, never silently delete — correct via a new entry or a human-approved edit.
- **Keep the Manifesto to one page.** Past two pages it's a rulebook — cut it back.
- **Eat the dogfood.** Run OS changes through the Feature Lifecycle and Decision Framework;
  log significant structural decisions in the Decision Framework's log.
- This repo is **docs-only** — no build, no tests. The gate is: does it follow the Manifesto,
  and is it dense, scannable, and stable?

---

## Standing behavioral rules (any agent, any repo under this OS)

The Manifesto, made agent-actionable:

- **One feature or bug at a time.** Explain the plan before broad edits.
- **Targeted edits only.** Don't rewrite whole files or refactor architecture unless the plan
  says so. Don't touch unrelated files "while you're here."
- **No new dependencies** without written justification and human approval (Manifesto §5).
- **"Done" means verified in the build you ship** (Manifesto §8) — not "it compiles," not
  "works in dev." Review your own diff; confirm you changed only what was agreed.
- **If a change breaks something, consider reverting before stacking fixes.**
- **Commit surgically** — name the files you mean to commit, never blanket `git add -A`
  (Lesson L-11).
- **Ask before changing scope.** AI builds; the human directs — vision, pillars, and taste
  stay the human's (Manifesto §10).
- **If context feels degraded, stop and write a handoff** instead of continuing — restart
  fresh from it.

## Off-limits

- Don't re-litigate settled entries in the Decision log without new evidence.
- Don't put words in the human's mouth in the Manifesto or Lessons Learned — flag inferences
  for review instead of asserting them as belief.
