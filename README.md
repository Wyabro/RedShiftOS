# RedShiftOS

**A practical operating system for building games with AI—without letting the process drift one prompt at a time.**

![RedShiftOS development system: prove, build, playtest, ship, and learn](assets/redshiftos-overview.svg)

RedShiftOS is a focused set of rules, workflows, and templates that travels with each game.
It keeps the human director and AI agents aligned on the same intent, architecture, current
task, and standard of proof.

It is not a game engine, framework, or code generator. Your game keeps its own source,
identity, and decisions. RedShiftOS supplies the repeatable development process around it.

## Why it exists

AI can produce code quickly. The harder problem is keeping many sessions pointed toward the
same game instead of letting each prompt create a locally sensible but globally expensive
change.

RedShiftOS provides the durable context to:

- prove that an idea is worth building before giving it production structure;
- keep one clear task, one owner for each responsibility, and a record of important choices;
- separate technical evidence from human judgment about whether the game is good;
- catch architectural drift and repeated mistakes before they spread; and
- verify work in the artifact players actually receive.

## Start here

### If you want to understand the system

1. Read the [Development Manifesto](FOUNDATION/Development-Manifesto.md) for the ten binding
   rules.
2. Read the [Feature Lifecycle](FOUNDATION/Feature-Lifecycle.md) for the path from idea to
   verified release.
3. Read [AGENTS.md](AGENTS.md) for the working loop and the task-specific reading map.

### If you are starting a game

1. Add RedShiftOS as a pinned git submodule at `./RedShiftOS` in the game repository.
2. Copy `AGENTS.md`, `PROJECT.md`, `Tasks.md`, and the thin pointer files from
   [`GAME_TEMPLATE/`](GAME_TEMPLATE/) into the game repository root.
3. Fill in `PROJECT.md` with the game's promise, pillars, constraints, and ownership
   boundaries.
4. Put one top task in `Tasks.md`.
5. Run the smallest concept proof that can answer one written question.

Game-local decisions, task state, provenance, and handoffs stay in the game repository.
RedShiftOS stays focused on reusable studio process.

## How work moves

Every non-trivial change follows the same visible loop:

> **Explore → Plan → Approve → Implement → Diff-review → Commit → Validate → Handoff**

The amount of ceremony scales with the risk. A documentation correction does not run the
same gates as a new gameplay system, but both must remain focused and verifiable. The
[Feature Lifecycle](FOUNDATION/Feature-Lifecycle.md#task-classes--match-the-gates-to-the-work)
defines the task classes and their required gates.

Across a whole game, work moves through seven phases:

> **Project → Design → Engineering → Implementation → Playtesting → Production → Postmortems**

Lessons from the end feed back into the foundation, so the process improves without turning
one game's history into permanent studio law.

## What is inside

| Area | What it gives you |
|---|---|
| [`AGENTS.md`](AGENTS.md) | Canonical agent rules, working loop, and task-based context map |
| [`FOUNDATION/`](FOUNDATION/) | Manifesto, philosophy, evidence gates, lessons, decisions, and anti-patterns |
| [`LIFECYCLE/`](LIFECYCLE/) | Guidance that belongs to a specific phase of making and shipping a game |
| [`AI/`](AI/) | Model routing, agent loops, prompt patterns, and recovery when work stalls |
| [`GAME_TEMPLATE/`](GAME_TEMPLATE/) | The small set of files that connects a new game to the OS |
| [`ROADMAP.md`](ROADMAP.md) | Current maturity, success measures, and automation that has not earned its place yet |

The repository intentionally avoids empty scaffolding. A document or directory appears only
when real work has given it something useful to hold.

## Current status

**v0.2 is an evidence kernel in field trial.** Its concept-proof and production-foundation
gates are ready to use, but the complete system is not yet proven across a full development
cycle. The [roadmap](ROADMAP.md) defines the evidence required before stronger claims or
automation are justified.

## License

RedShiftOS is available under the [MIT License](LICENSE).
