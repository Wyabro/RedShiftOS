# AGENTS.md — RedShiftOS

**Canonical rules for working with RedShiftOS. Read this first, every session.** `CLAUDE.md`
points here; other agents (Cursor, Copilot, Windsurf, Cline) should be pointed here too — the
filename varies, the habit doesn't: the rules live in the repo and every session reads them
first.

RedShiftOS is an AI-first **Game Development Operating System** — a docs/process repo, not
code. It carries how Red Shift Studios builds games so every agent pulls toward the same north
star instead of optimizing one prompt at a time. It lives *beside* every game.

---

## Always load (the whole session-start budget)

Two things, every session — nothing else:

1. **This file** — the working loop, the non-negotiables, and the task map below.
2. **`FOUNDATION/Development-Manifesto.md`** — the 10 rules (binding; they override your defaults).

Everything else in `FOUNDATION/` is a **reference law library** — pull it *by task*, don't
boot-load it. "More context is not better context — it dilutes signal" (`AI/Model-Routing.md`).
A syllabus you skim beats nothing, but loses to a single page you actually follow.

## Load by task

Pull only what the task in front of you needs:

| When you're… | Also load |
|---|---|
| starting a new system / feature | `FOUNDATION/Feature-Lifecycle.md` (full) · `FOUNDATION/Decision-Framework.md` · Lessons tagged *architecture / design* |
| fixing a bug | root-cause first (L-14) · the Lesson IDs for that area (Lessons → "Pull by tag") · the proof ladder |
| tuning feel / balance | the prototype + playtest gates · Lessons tagged *design / playtest* |
| working netcode / multiplayer | Lessons tagged *net* (L-02, L-08) · the game's own netcode notes |
| shipping or verifying | the proof ladder (`FOUNDATION/Feature-Lifecycle.md`) · `LIFECYCLE/6-PRODUCTION/Assets-and-Provenance.md` |
| reviewing a diff | `FOUNDATION/Anti-Patterns.md` — the detection checklist |
| an agent is stuck / looping | `AI/Triage-and-Recovery.md` |
| picking a model / watching cost | `AI/Model-Routing.md` |
| a rule and reality conflict | `FOUNDATION/Studio-Philosophy.md` — the *why* under the rules |
| ending a session | write a handoff → `LIFECYCLE/4-IMPLEMENTATION/Session-Handoffs.md` |

When a lesson or decision names a file, flag, or command, verify it still exists before
relying on it — these are point-in-time notes, not live state.

---

## The working loop

Every non-trivial change follows this loop — it's what keeps agents consistent:

**Explore → Plan → Approve → Implement → Diff-review → Commit → Validate → Handoff.**

- **Explore** the code and report risks — no edits yet.
- **Plan** in writing: files to touch, approach, risks, how it'll be verified.
- **Human approves the plan.** This checkpoint is the highest-leverage gate — it's cheaper to
  fix a plan than a diff.
- **Implement** the plan, targeted edits only.
- **Diff-review:** the human reads the diff. For non-trivial work, a *fresh-context*
  writer-reviewer checks the diff + plan for scope creep — the agent that wrote the code has
  already convinced itself it's fine.
- **Commit** as a save-state (checkpoint before risky runs; feature branches — Lesson L-11).
- **Validate** against both merge gates + the proof ladder (`FOUNDATION/Feature-Lifecycle.md`).
- **Handoff** (`LIFECYCLE/4-IMPLEMENTATION/Session-Handoffs.md`) so the next session doesn't start from zero.

Never go straight from request to code. Scale the ceremony to the task class (see the Feature
Lifecycle) — a typo fix and a new game mode do not get the same gates.

## Standing non-negotiables (any agent, any repo under this OS)

The Manifesto, made agent-actionable:

- **One feature or bug at a time** — the single top task in `Tasks.md`. The human sets
  priority by editing that file; don't self-assign what to work on. Explain the plan before
  broad edits.
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

---

## If you're building a GAME with this OS

- RedShiftOS is checked out **beside your game repo** — a git submodule at `./RedShiftOS`, or
  a sibling directory. At session start, load the Manifesto + this task map, then pull
  FOUNDATION by task.
- The game repo has its **own** `AGENTS.md` + thin `CLAUDE.md` that point here — start from
  `GAME_TEMPLATE/` (copy `AGENTS.md` + `PROJECT.md` into the new repo).
- Session load order inside a game: game `AGENTS.md` → the Manifesto → *what the task needs* →
  the game's `PROJECT.md` (its contract) → the game's current tasks / status / latest handoff.
- **Game-specific decisions and burns live in the game repo** (a `decisions.md` / postmortem).
  Promote a lesson into RedShiftOS only when a *second* game re-derives it — that's what keeps
  the OS evergreen instead of one game's archive.
- Non-trivial features run through `FOUNDATION/Feature-Lifecycle.md` — pick the task class, run
  its gates. Prove it before you build it.

## If you're working ON RedShiftOS itself

- **No stub files.** A document exists only when it holds real content (Decision #1). Empty
  scaffolding is the exact sprawl this OS exists to prevent.
- **Lessons-Learned IDs are append-only and stable.** A new lesson gets the next `L-NN`;
  never renumber, never silently delete — correct via a new entry or a human-approved edit.
- **Keep the always-load surface small.** The Manifesto stays one page; the task map stays a
  table. New depth goes into reference docs pulled by task, never into the boot budget.
- **Eat the dogfood.** Run OS changes through the Feature Lifecycle and Decision Framework;
  log significant structural decisions in the Decision Framework's log.
- This repo is **docs-only** — no build, no tests. The gate is: does it follow the Manifesto,
  and is it dense, scannable, and stable?

## Off-limits

- Don't re-litigate settled entries in the Decision log without new evidence.
- Don't put words in the human's mouth in the Manifesto or Lessons Learned — flag inferences
  for review instead of asserting them as belief.
