# RedShiftOS

**An AI-first operating system for planning, architecting, building, testing, and shipping games.**

RedShiftOS is not a game and not an engine. It is the repeatable development *process* —
design philosophy, engineering rules, production workflow, and AI-collaboration standards —
that lives beside every game RedShift Studios makes. The code changes from project to
project. The process shouldn't.

It exists because of one lesson from the first game (Cart Clash): every time an AI agent
implemented a feature, it only knew about *that feature*. It didn't know the long-term
vision, the design pillars, the architectural intent, or the lessons already learned. So
every implementation was locally optimal and globally expensive.

RedShiftOS is the shared context that fixes that. Before an agent touches a new game's
source tree, it reads this repository first — and now it knows *how RedShift builds games*,
not just *today's prompt*.

## How it's used

1. A new game starts by ingesting RedShiftOS — `FOUNDATION/` first, then `PROCESS/`.
2. Every feature flows through the [Feature Lifecycle](PROCESS/Feature-Lifecycle.md). No skipping.
3. Every significant decision is run through the [Decision Framework](FOUNDATION/Decision-Framework.md) and logged.
4. Every project's pain feeds back into [Lessons Learned](FOUNDATION/Lessons-Learned.md). The system gets smarter because we actually shipped with it.

## v0.1 scope

Deliberately tiny. The foundation only. No templates, no engine guides, no prompt library,
no empty folders — those get added when they have real content to hold.

- [x] README (the "why")
- [x] [Development Manifesto](FOUNDATION/Development-Manifesto.md) — how RedShift builds games, on one page
- [x] [Lessons Learned](FOUNDATION/Lessons-Learned.md) — Cart Clash pain turned into permanent rules
- [x] [Decision Framework](FOUNDATION/Decision-Framework.md) — the checklist before any major commitment
- [x] [Feature Lifecycle](PROCESS/Feature-Lifecycle.md) — the path every feature follows, idea → merge

## Structure

```
RedShiftOS/
├── README.md
├── AGENTS.md            ← canonical agent rules; read first, every session
├── CLAUDE.md            ← thin pointer to AGENTS.md (Claude Code reads this)
├── FOUNDATION/
│   ├── Development-Manifesto.md
│   ├── Lessons-Learned.md
│   └── Decision-Framework.md
├── PROCESS/
│   ├── Feature-Lifecycle.md
│   └── Session-Handoffs.md
├── AI/
│   ├── Model-Routing.md
│   ├── Triage-and-Recovery.md
│   └── Prompt-Library.md
├── REFERENCE/
│   └── Assets-and-Provenance.md
└── GAME_TEMPLATE/       ← copy into a new game repo to wire it to this OS
    ├── AGENTS.md
    ├── PROJECT.md
    ├── Tasks.md
    └── README.md
```

## The system eats its own dogfood

RedShiftOS is built using RedShiftOS. Its first "feature" is this foundation, and it went
through the same Feature Lifecycle and Decision Framework it prescribes. If the process
feels cumbersome while building the OS, it would feel cumbersome while building a game —
and that's the signal to fix the process, not to skip it.

---

*Status: v0.1 foundation complete, agent wiring in place, and the best of the ai-builder-
playbook folded in — verification gates + proof ladder, the agent working-loop, an `AI/` layer
(model routing, triage & recovery, prompt library), and asset provenance. The Manifesto is in
Wyatt's voice; Lessons Learned is seeded (17) and grows as games ship. Remaining folders
(TEMPLATES, POSTMORTEMS) earn their place when Game #2 gives them real content.*
