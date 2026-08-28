# RedShiftOS

**The operating system for building better games — an AI-first process for planning,
architecting, building, testing, and shipping.**

![RedShiftOS — the operating system for building better games](assets/redshiftos-overview.svg)

RedShiftOS is not a game and not an engine. It is the repeatable development *process* —
philosophy, engineering rules, production workflow, and AI-collaboration standards — that
lives beside every game Red Shift Studios makes. **The code changes from project to project.
The process shouldn't.**

It exists because of one lesson from the first game (Cart Clash): every time an AI agent
implemented a feature, it only knew about *that feature*. It didn't know the long-term
vision, the design pillars, the architectural intent, or the lessons already learned. So
every implementation was locally optimal and globally expensive.

RedShiftOS is the shared context that fixes that. Before an agent touches a new game's
source tree, it reads this repository first — and now it knows *how Red Shift Studios builds
games*, not just *today's prompt*.

## How it's used

1. **Load the Manifesto + `AGENTS.md`'s task map** — then pull the rest of `FOUNDATION/` *by
   task*. Don't boot-load the whole library; more context dilutes signal.
2. **Move through the lifecycle phases** as you build — Project → Design → … → Postmortems
   (`LIFECYCLE/`).
3. **Non-trivial features run the [Feature Lifecycle](FOUNDATION/Feature-Lifecycle.md)** — pick
   the task class, run its gates.
4. **Every significant decision** runs the [Decision Framework](FOUNDATION/Decision-Framework.md)
   and gets logged (game-specific ones live in the game repo).
5. **Every shipped project's pain** feeds back into
   [Lessons Learned](FOUNDATION/Lessons-Learned.md) and
   [Anti-Patterns](FOUNDATION/Anti-Patterns.md). The system gets smarter because we actually
   shipped with it.

## What's inside

- **[FOUNDATION/](FOUNDATION/)** — how we think: [Philosophy](FOUNDATION/Studio-Philosophy.md)
  (the *why*), [Manifesto](FOUNDATION/Development-Manifesto.md) (the *what*),
  [Lessons Learned](FOUNDATION/Lessons-Learned.md), [Anti-Patterns](FOUNDATION/Anti-Patterns.md),
  [Decision Framework](FOUNDATION/Decision-Framework.md), and the
  [Feature Lifecycle](FOUNDATION/Feature-Lifecycle.md).
- **[LIFECYCLE/](LIFECYCLE/)** — the seven phases; folders exist where content does, the rest
  are covered by FOUNDATION until a game fills them.
- **[AI/](AI/)** — the cross-cutting AI-collaboration layer: model routing, triage & recovery,
  prompt library, [agent loops](AI/Agent-Loops.md).
- **[GAME_TEMPLATE/](GAME_TEMPLATE/)** — copy into a new game repo to wire it to this OS.
- **[ROADMAP.md](ROADMAP.md)** — where this is headed (the automation endgame — recorded, not
  yet built).

## Structure

```
RedShiftOS/
├── README.md
├── AGENTS.md              ← canonical agent rules; read first, every session
├── GROK.md / CLAUDE.md / GEMINI.md / .cursorrules  ← thin pointers; do not restate AGENTS.md
├── ROADMAP.md             ← where this is headed
├── FOUNDATION/            how we think — Manifesto always-load; the rest by task
│   ├── Studio-Philosophy.md
│   ├── Development-Manifesto.md
│   ├── Lessons-Learned.md
│   ├── Anti-Patterns.md
│   ├── Decision-Framework.md
│   └── Feature-Lifecycle.md
├── LIFECYCLE/             the 7 phases (folders exist where content does — see LIFECYCLE/README)
│   ├── 1-PROJECT/         define the game
│   ├── 3-ENGINEERING/     engine cuts (+ Godot.md, 4.7.x)
│   ├── 4-IMPLEMENTATION/  build it (+ Session-Handoffs.md)
│   └── 6-PRODUCTION/      ship it (+ Assets-and-Provenance.md)
├── AI/                    cross-cutting AI-collaboration layer
│   ├── Model-Routing.md
│   ├── Triage-and-Recovery.md
│   ├── Prompt-Library.md
│   └── Agent-Loops.md
└── GAME_TEMPLATE/         copy into a new game repo to wire it to this OS
    ├── AGENTS.md
    ├── GROK.md / CLAUDE.md / GEMINI.md / .cursorrules
    ├── PROJECT.md
    ├── Tasks.md
    └── README.md
```

## The system eats its own dogfood

RedShiftOS is built using RedShiftOS. Its changes run through the same Feature Lifecycle and
Decision Framework it prescribes, and its structural calls are logged in the Decision
Framework. If the process feels cumbersome while building the OS, it would feel cumbersome
while building a game — and that's the signal to fix the process, not to skip it.

---

*Status: **v0.2 evidence kernel.** Game #2 (Slop Park) is frozen — do not polish it. The OS
freeze from Decision #5 is lifted (Decisions #6–#8): evidence states, L-18–L-22, Godot 4.7
cuts, the Production Foundation Gate after KEEP, and thin per-tool pointers so every model
hits the same `AGENTS.md`. Measure on Game #3 — metrics in [ROADMAP.md](ROADMAP.md). Still no
automation wizard.*
