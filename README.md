# RedShiftOS

**The operating system for building better games — an AI-first process for planning,
architecting, building, testing, and shipping.**

![RedShiftOS — the operating system for building better games](assets/redshiftos-overview.png)

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

The OS walks you through a game in order:

1. **Read `FOUNDATION/` first** — how the studio thinks. It's binding and applies to every phase.
2. **Walk `LIFECYCLE/` in order** — Project → Design → Engineering → Implementation →
   Playtesting → Production → Postmortems.
3. **Every feature flows through the [Feature Lifecycle](FOUNDATION/Feature-Lifecycle.md)** —
   idea → merge, no skipping.
4. **Every significant decision** runs the [Decision Framework](FOUNDATION/Decision-Framework.md)
   and gets logged.
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
- **[LIFECYCLE/](LIFECYCLE/)** — the seven chronological phases, each with its own guidance.
- **[AI/](AI/)** — the cross-cutting AI-collaboration layer: model routing, triage & recovery,
  prompt library.
- **[GAME_TEMPLATE/](GAME_TEMPLATE/)** — copy into a new game repo to wire it to this OS.
- **[ROADMAP.md](ROADMAP.md)** — where this is headed (the automation endgame — recorded, not
  yet built).

## Structure

```
RedShiftOS/
├── README.md
├── AGENTS.md              ← canonical agent rules; read first, every session
├── CLAUDE.md              ← thin pointer to AGENTS.md (Claude Code reads this)
├── ROADMAP.md             ← where this is headed
├── FOUNDATION/            how we think — loaded first, applies to every phase
│   ├── Studio-Philosophy.md
│   ├── Development-Manifesto.md
│   ├── Lessons-Learned.md
│   ├── Anti-Patterns.md
│   ├── Decision-Framework.md
│   └── Feature-Lifecycle.md
├── LIFECYCLE/             the chronological phases — the OS walks you through a game
│   ├── 1-PROJECT/
│   ├── 2-DESIGN/
│   ├── 3-ENGINEERING/
│   ├── 4-IMPLEMENTATION/  (+ Session-Handoffs.md)
│   ├── 5-PLAYTESTING/
│   ├── 6-PRODUCTION/      (+ Assets-and-Provenance.md)
│   └── 7-POSTMORTEMS/
├── AI/                    cross-cutting AI-collaboration layer
│   ├── Model-Routing.md
│   ├── Triage-and-Recovery.md
│   └── Prompt-Library.md
└── GAME_TEMPLATE/         copy into a new game repo to wire it to this OS
    ├── AGENTS.md
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

*Status: foundation complete and organized around the development lifecycle. FOUNDATION holds
philosophy, manifesto, 17 lessons, a 13-entry anti-pattern catalog, the decision log, and the
feature pipeline; LIFECYCLE lays out the seven phases; AI/ carries the collaboration layer.
Phase folders and future layers grow as real games — starting with Game #2 — give them
content. See [ROADMAP.md](ROADMAP.md).*
