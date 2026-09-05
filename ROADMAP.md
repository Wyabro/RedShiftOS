# Roadmap

*Where RedShiftOS is headed. This records the vision so it isn't lost — it is **not** a to-do
list to build now. The rule stands: nothing here gets built until a real game needs it
(Decision #1). The OS is docs today; it earns tooling later.*

---

## Now — v0.2 velocity-correction field trial (Decisions #6–#11)

Game #2 (Slop Park) ran the v0.1 OS and dead-ended. Agents stamped PASS; the human gate is
FAIL. The project is **frozen** — do not polish it.

v0.2’s job is to **reject weak hooks quickly** (one-hypothesis prototypes, evidence states,
Godot 4.7 hard cuts, ranked agent loops). KEEP opens the Production Foundation Gate
(Decision #7) — not production feature cards. **Still no automation wizard.** Pointers stay
thin (Decision #8) so Grok, Codex, Cursor, Gemini, and Claude hit the same `AGENTS.md`.

**Status:** GAME-3 produced useful but mixed evidence. Human-only verdicts stayed controlled,
the Production Foundation Gate completed, reuse-first rules corrected an arbitrary dependency
cap, and real playable systems reached artifact and network proof. The trial also showed that
serial approvals, excess records, repeated context reconstruction, and copied volatile state
can delay the next playable build. The product concept still required correction. RedShiftOS is
not production-proven. The supported layout remains a git submodule at `./RedShiftOS`, pinned to
a named commit. Change the pin only through a named game decision plus a cold-start smoke test.

**Next trial goal:** reach the next human-playable build quickly and recover cleanly between
sessions. Measure the approved iteration grant, the active-state source of truth, and one
bounded support lane without weakening human verdicts, ownership boundaries, or artifact proof.

**How we'll measure the next throughput trial:**

- **False PASS** — agent writes HUMAN PASS / KEEP. *(zero)*
- **Repeat L-18–L-22** — critic-as-close, Box3D-class plugins, OS-before-laugh, harness-as-game, solo-only prove on a co-op 1.0. *(fewer)*
- **Time to first human play** of the core verb. *(hours, not days)*
- **OS overhead** — hours maintaining the OS vs. hours in `prototypes/`. *(small)*
- **Playable-build cadence** — approved slices that reach the named next build before new
  process records accumulate. *(increases)*
- **Recovery clarity** — new sessions find the active task and evidence state from `Tasks.md`
  and the latest handoff without oral correction. *(yes)*
- **Support-lane safety** — any parallel support work stays inside its allowlist and passes
  focused integration review. *(yes or lane removed)*

If these don't move, cut v0.2 harder — do not add layers.

---

## The five layers

RedShiftOS grows through five layers. The first three exist; the last two are ahead.

1. **Philosophy** — why we build this way. *(FOUNDATION: Studio-Philosophy, Manifesto.)* — ✅
2. **Process** — what happens, in what order, who reviews. *(FOUNDATION: Feature-Lifecycle,
   Decision-Framework; the LIFECYCLE phases; evidence states.)* — ✅
3. **Standards** — folder layouts, code, scenes, naming, git, AI. *(Partial: AGENTS, AI/,
   Anti-Patterns, `LIFECYCLE/3-ENGINEERING/Godot.md` for Godot 4.7.)* — ◐
4. **Templates** — every document already exists; copy, rename, fill. *(Partial: GAME_TEMPLATE;
   prove-then-structure + thin pointers.)* — ◐
5. **Automation** — the OS *generates* the work, not just describes it. — ○

## The automation endgame

The templates are the *manual* version of a wizard. The endgame is a **"Create Feature"** step
that, from one intent, generates the design doc, risk analysis, architecture review,
implementation plan, QA checklist, playtest plan, and merge checklist — each pre-filled with
this OS's standards and the game's `PROJECT.md` context.

Further out, a **Studio Kernel / New-Project Wizard**: one command that creates the game repo,
wires RedShiftOS in, copies templates, generates AI context, opens the decision log and the
feature/playtest trackers, and bootstraps the engine — so you're not creating a game, you're
creating a studio-ready project.

## Why not the wizard now

Automation is tooling, and the OS is docs. Building the wizard before a game **past KEEP**
names a repetitive step is Architecture Astronautics (`FOUNDATION/Anti-Patterns.md`). Game #2
showed we needed gates, not generators. Evidence first, tooling second.
