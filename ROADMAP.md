# Roadmap

*Where RedShiftOS is headed. This records the vision so it isn't lost — it is **not** a to-do
list to build now. The rule stands: nothing here gets built until a real game needs it
(Decision #1). The OS is docs today; it earns tooling later.*

---

## Frozen — validating on Game #2

The OS is a hypothesis: that loading this context makes agents build better games. Game #2 is
the experiment. **No OS expansion — no new standards, no new phases, no automation — until
Game #2 produces evidence.** Build the game *by hand* through the OS, feel what's actually
missing, and let that drive the next change.

**How we'll know it's working** (measure on Game #2 against the Cart Clash baseline):

- **Repeat mistakes** — times we re-hit a Cart Clash lesson class the OS already names. *(fewer)*
- **Scope creep** — times an agent ran past its approved plan. *(fewer — the plan gate should catch them)*
- **Done → verified gap** — median time from an agent saying "done" to verified-in-the-shipped-artifact. *(shrinks — the proof ladder should make it routine)*
- **OS overhead** — hours maintaining the OS vs. hours coding the game. *(small, and falling)*

If these don't move, the OS is lore, not leverage — and we cut harder, not add.

---

## The five layers

RedShiftOS grows through five layers. The first three exist; the last two are ahead.

1. **Philosophy** — why we build this way. *(FOUNDATION: Studio-Philosophy, Manifesto.)* — ✅
2. **Process** — what happens, in what order, who reviews. *(FOUNDATION: Feature-Lifecycle,
   Decision-Framework; the LIFECYCLE phases.)* — ✅
3. **Standards** — folder layouts, code, scenes, naming, git, AI. *(Partial: AGENTS, AI/,
   Anti-Patterns; a `LIFECYCLE/3-ENGINEERING` folder gets created when Game #2 produces real standards.)* — ◐
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
