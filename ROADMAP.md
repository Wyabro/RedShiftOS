# Roadmap

*Where RedShiftOS is headed. This records the vision so it isn't lost — it is **not** a to-do
list to build now. The rule stands: nothing here gets built until a real game needs it
(Decision #1). The OS is docs today; it earns tooling later.*

---

## The five layers

RedShiftOS grows through five layers. The first three exist; the last two are ahead.

1. **Philosophy** — why we build this way. *(FOUNDATION: Studio-Philosophy, Manifesto.)* — ✅
2. **Process** — what happens, in what order, who reviews. *(FOUNDATION: Feature-Lifecycle,
   Decision-Framework; the LIFECYCLE phases.)* — ✅
3. **Standards** — folder layouts, code, scenes, naming, git, AI. *(Partial: AGENTS, AI/,
   Anti-Patterns; `LIFECYCLE/3-ENGINEERING` is where these land.)* — ◐
4. **Templates** — every document already exists; copy, rename, fill. *(Partial: GAME_TEMPLATE;
   grows into per-phase templates — design doc, risk analysis, QA checklist, playtest plan.)* — ◐
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

## Why not now

Automation is tooling, and the OS is docs. Building the wizard before there's a second game to
run it on is Architecture Astronautics (see `FOUNDATION/Anti-Patterns.md`). The path is: build
Game #2 *by hand* through this OS, feel exactly which steps are repetitive and painful, and
automate those — evidence first, tooling second. The OS is more valuable than any one game;
let it earn its automation from real use.
