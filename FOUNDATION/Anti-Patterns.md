# Anti-Patterns

*A catalog of named failure modes a solo dev and their AI agents fall into — the recurring
smells, not the stories. Lessons-Learned is narrative (what broke on Cart Clash → the rule);
this is a lookup table of shapes to pattern-match against while you work. When you or an agent
recognizes one here, stop, apply Recovery/Prevention, and follow the cross-ref to the Lesson
for the "why." Detection lines are written to be checkable — grep-able, countable, or a
one-sentence test — so an agent can self-audit a diff before a human ever reads it.*

> **Seeded from the Cart Clash build**, cross-referenced to Lessons-Learned (L-NN) and the
> Manifesto (§N). Real instances are named; where Cart Clash has none on record, the entry is
> marked "(general)" and kept honest — a smell to watch, not a defect to claim. Wyatt: verify,
> correct, add the ones only you've hit.

---

## How to use this

- **Before writing:** skim the Smell Index. If your task touches a listed hotspot (a big file,
  the netcode, the menu, `CONFIG`), read that entry first.
- **During diff-review:** run each entry's Detection line against the diff. The agent that
  wrote the code has already convinced itself it's fine — this is the checklist that doesn't.
- **When one matches:** it is not automatically a bug. Decide if it's load-bearing or debt,
  then apply Recovery (dig out) or Prevention (stop the next one). Log the call if it's a real
  decision (Decision-Framework).

## Smell index

| # | Anti-pattern | One-line tell | Ref |
|---|---|---|---|
| 1 | God Files | Describing the file needs the word "and" | L-04 |
| 2 | Hidden State | The fix was a comment warning the next editor | L-02, L-08 |
| 3 | Temporal Coupling | "Call A must run before B" — and nothing enforces it | L-02, L-12 |
| 4 | Magic Numbers | A bare number in logic; the same literal in two files | L-17 |
| 5 | Feature Creep | Can't name the pillar it serves | §6 |
| 6 | One-Off Exceptions | `if (arena === 'x')` inside generic code | L-13, L-17 |
| 7 | Premature Optimization | Optimized before a profile/playtest asked for it | L-09 |
| 8 | Refactoring Without Purpose | Diff touches files the task didn't name | L-11 |
| 9 | Dependency Collecting | A dependency earning its keep in one call site | §5 |
| 10 | Menu Explosion | The menu file is a top-3 churn hotspot | §6, L-06 |
| 11 | Singleton Abuse | Runtime code writes to a global / `CONFIG` | §4 |
| 12 | Architecture Astronautics | An abstraction with exactly one implementation | §5 |
| 13 | Manager-Manager Syndrome | A Manager whose job is managing Managers | §4 |

---

### 1. God Files
- **Symptoms:** One file you're afraid to touch. Every feature "just adds a bit here." Merge
  conflicts and reentrancy bugs cluster in it. New agents burn half their context just loading it.
- **Causes:** Responsibilities accreted faster than they were extracted; the file was organized
  by feature history, not by responsibility. Agents append to the file they're already in.
- **Consequences:** Unreviewable diffs, interleaving bugs (scoring ↔ round-end ↔ physics-body
  removal touching each other mid-iteration), and blocked wins downstream (you can't code-split
  what you can't separate).
- **Detection:** Describing the file needs an "and." Or: it's your repo's top git-churn hotspot,
  or it's >~800 lines, or it imports from >3 unrelated domains. `git log --format= --name-only | sort | uniq -c | sort -rn | head` — the top entry is your prime suspect.
- **Recovery:** Carve a composition seam first (extract the smallest single-responsibility owner
  that has a clean interface), land it behind green tests, repeat. Don't attempt the big-bang split.
- **Prevention:** Extract on the *second* reason to open a file, not the fifth. One responsibility
  per file, enforced at review (§4).
- **Cart Clash / cross-ref:** `main.js` (5022 lines) and `simulation.js` (2966 lines) are the
  living examples; **MAIN-1** (carve the `main.js` seam) is a tracked debt item that *blocks*
  **BUNDLE-1** (menu/game code-split). The P0 reentrancy crash traced to scoring/round-end/body-
  removal living close enough to interleave. **L-04**, and see L-03 (boundaries are load-bearing).

### 2. Hidden State
- **Symptoms:** A system breaks when you change an *unrelated* one. Bugs get "fixed" by adding a
  comment warning the next person. Behavior depends on something set far away with no signal at
  the read site.
- **Causes:** State written in one place and read in another with no type, event, or invariant
  connecting them; platform state assumed to be fresh when it isn't; a global mutated at runtime.
- **Consequences:** Whole bug classes that are invisible until they bite (netcode stalls, stale
  rooms, silent no-ops), and fixes that don't hold because the state is still reachable.
- **Detection:** The fix for the last bug here was a comment, not a guardrail. Or: a value is set
  in module A and depended on in module B with nothing in B that references A. Or: "works in dev,
  silently does nothing / stale in prod." Count the append-only "gotchas" a file needs — that's
  its hidden-state debt.
- **Recovery:** Make the state explicit — an event, an owned setter, an assertion at the read
  site. Where it's platform state, add your own liveness/reset (heartbeat + reaper) instead of
  trusting the platform's word.
- **Prevention:** Prefer passing state to reaching for it. Treat "the connection is live" / "the
  deploy reset it" as a hypothesis you confirm, never a fact you're handed.
- **Cart Clash / cross-ref:** Cloudflare DO state persists across deploys (deploy ≠ reset) and
  "connected" wasn't liveness (**L-08**); `material.envMapIntensity` is a silent no-op against
  `scene.environment`; `visibilityState: hidden` freezes the sim even with `?perfPump`; the
  whole append-only STATUS "gotchas" list is a hidden-state ledger. **L-08**, **L-02**.

### 3. Temporal Coupling
- **Symptoms:** "This has to run before that." Reordering two innocuous lines breaks the game.
  Setup and teardown are split across files. A `microtask`/`setTimeout(0)`/`defer` exists purely
  to fix ordering, with a comment explaining why.
- **Causes:** Two pieces of code share an implicit sequence contract that the type system and
  the call sites don't express. Common where a value is produced by one system and consumed by
  another a frame/tick later.
- **Consequences:** Fragile seams; the classic "each fix is correct, the batch ships new bugs"
  crop; agents refactor for tidiness and silently reorder a load-bearing sequence.
- **Detection:** You can name a "must happen before" that nothing in the code enforces. Or: a
  defer/flush/order comment is the only thing keeping it alive. Or: swapping two adjacent
  statements changes behavior.
- **Recovery:** Encode the order as data or a state machine (explicit phases), single-source any
  value both ends must agree on, and add a regression test that asserts the sequence — not the
  symptom.
- **Prevention:** When two systems share a timeline, make the dependency a visible contract
  (one owner, one ordered pipeline), and run an integration pass after any batch of fixes that
  targets the *seams*, not the individual diffs.
- **Cart Clash / cross-ref:** `match_ended` must microtask-defer because of netcode setter
  order; the EffectComposer pass order is load-bearing (`toneMapping` is a no-op into composer
  RTs without `OutputPass`); server and client must agree on `COUNTDOWN_MS`; a TDZ regression
  came from a declaration-order slip (`pendingArenaRotationLevelId`). **L-02**, **L-12**.

### 4. Magic Numbers
- **Symptoms:** Bare literals scattered through logic. Tuning means editing code and rebuilding.
  The same constant appears in two files and has to be kept in sync by hand.
- **Causes:** A number gets typed in "for now" and never named; feel constants live where they're
  first used instead of in one config; duplication feels cheaper than a shared import.
- **Consequences:** Un-tunable feel, drift between duplicated copies (the two silently disagree),
  and numbers whose meaning is lost the moment the author forgets them.
- **Detection:** A number in a conditional/branch with no named constant. Or: `grep` the literal
  and it appears in >1 file. Or: to change the feel you edit source instead of data. Or: the
  number needs a comment to explain what it is.
- **Recovery:** Hoist to a named constant in one config module; for anything you tune by feel,
  wire it to a live panel so you tune without rebuilding; single-source any value two systems
  must agree on.
- **Prevention:** A number that appears in logic gets a name on first write; a number two places
  need gets one home and an import. Feel constants belong in config from the start.
- **Cart Clash / cross-ref:** The `?tune` Tweakpane panel exists *precisely because* feel
  constants (driving/boost/ram/cargo/edge) were scattered — it edits `CONFIG` live and exports a
  diff. The duplicated countdown was collapsed to a shared `COUNTDOWN_MS = 4500` (server + client
  had to agree). Recurring "magic number in two places" is exactly the class L-17 says to close
  with an invariant. **L-17**.

### 5. Feature Creep
- **Symptoms:** Scope larger than the design doc. "While we're at it." Features nobody asked for
  sitting next to the ones that matter. The backlog grows faster than it drains.
- **Causes:** Every idea feels cheap in isolation; a solo dev is their own unchecked product
  owner; agents happily build whatever's described without asking what it's *for*.
- **Consequences:** Diluted focus, more surface to maintain and break, polish spread thin across
  things that don't serve a pillar.
- **Detection:** You can't name the design pillar a feature serves, or the feeling it creates.
  Or: the feature isn't in the design doc and no decision-log entry explains its arrival.
- **Recovery:** Cut what doesn't pull its weight. Run laggards back through the Player-Value gate;
  if it serves no pillar, it goes.
- **Prevention:** Every feature runs the Feature-Lifecycle Player-Value gate before it earns
  production structure — name the pillar and the feeling, or stop. "One top task at a time."
- **Cart Clash / cross-ref:** (general — Cart Clash mostly held the line via passes, but the
  Living Store + PA directives layered real new surface; DIR-1 is the debt it left.) **§6**,
  and Feature-Lifecycle's Player-Value gate.

### 6. One-Off Exceptions
- **Symptoms:** Special-case branches threaded through generic code — `if (arena === 'x')`,
  `if (isSpecial)`. Each new variant means another edit in the same N places. Behavior forks by
  name instead of by data.
- **Causes:** A single exception is faster to hardcode than to generalize; then a second appears,
  then a third. Agents add the branch in front of them rather than the abstraction behind it.
- **Consequences:** Adding variant N+1 requires touching every fork; the special-cases drift out
  of sync; the "generic" system is a lie.
- **Detection:** A name-checked conditional (`=== 'zanzibar'`) inside otherwise-generic logic.
  Or: count the special-cases for a concept — if it's ~one per variant, it's a data table wearing
  a code costume.
- **Recovery:** Lift the exceptions into a pure-data catalog the generic code reads; the special-
  cases become rows, not branches. Add a variant by adding data, not code.
- **Prevention:** On the *second* special-case, stop and make it data. When the same class keeps
  recurring, move the rule into one enforced place (L-17).
- **Cart Clash / cross-ref:** Per-arena special-cases accreted — `arenaExposureMul{zanzibar:1.18}`,
  VHS gated to Storerooms only, per-arena bloom, Sundial high-ground bonus, floor mats getting
  their own `envMap` at 0.25× to dodge the `envMapIntensity` no-op. The AI-hazard case is L-13
  (one arena's hole had no path routing because logic didn't generalize). **L-13**, **L-17**.

### 7. Premature Optimization
- **Symptoms:** Effort spent making something fast before anything proved it was slow. An
  optimization shipped as the default with no fallback. Time sunk chasing a number that doesn't
  cost the player anything.
- **Causes:** Optimizing is more fun than validating; a shiny faster-path looks free; nobody put
  a before/after number on it.
- **Consequences:** Game-breaking regressions from unproven paths, wasted time on non-problems,
  and dev-only artifacts mistaken for prod bugs.
- **Detection:** No profile or playtest pointed at this before it was optimized. Or: the fast
  path is the default with no working fallback behind a flag. Or: there's no measured before/after.
- **Recovery:** Revert the unproven optimization to opt-in behind a fallback; keep it only if a
  measurement justifies it. Put a number on the thing before touching it again.
- **Prevention:** Don't optimize before a playtest/profile asks for it; never make an unproven
  optimization the default; every optimization is a dependency that must degrade gracefully.
- **Cart Clash / cross-ref:** SIMD Rapier was made the default (`9d8a69e`), shipped a borrow
  error, and was reverted to standard-by-default / opt-in only (`8174180`). Separately, a scary
  "2s first level-swap" was a Vite dev-transform artifact worth zero prod cost — dev noise
  masquerading as a prod bug. **L-09**, and L-14 (put a number on it).

### 8. Refactoring Without Purpose
- **Symptoms:** A diff touches files the task never named. "Cleanup" with no behavior change and
  no ticket. Architecture rewrites that arrive uninvited, often right when you can least afford
  the risk.
- **Causes:** Tidiness is satisfying; agents "improve" code while passing through it; the urge to
  restructure before the current work is even validated.
- **Consequences:** Blast radius far beyond the task, new seam bugs, review noise that hides the
  real change, and destabilization right before a gate.
- **Detection:** The diff's file list ⊄ the plan's file list. Or: "while I was here." Or: a
  structural refactor landing *before* the validation gate it could break.
- **Recovery:** Split the incidental refactor out; revert it from the task branch; land it (if
  worth it) as its own reviewed change with its own justification.
- **Prevention:** Targeted edits only — name the files up front and touch nothing else. Structural
  debt waits until the current gate is green, then gets its own plan.
- **Cart Clash / cross-ref:** (general — the standing rules exist because of it.) Structural debt
  (MAIN-1, DIR-1, GLTF-1, TS-1) is deliberately parked *behind* the NET-1 validation gate rather
  than chased mid-flight. **L-11** (surgical commits / feature branches), AGENTS standing rules.

### 9. Dependency Collecting
- **Symptoms:** A dependency pulling its weight in one call site. `package.json` growing without
  a paper trail. Something you could hand-roll in a dozen lines pulled in as a library.
- **Causes:** A dep is the fast answer to a small problem; agents add imports freely; nobody
  weighs the lifetime cost against the one-time convenience.
- **Consequences:** Bundle bloat, supply-chain and version-pin surface, and coupling to code you
  don't control — every dependency is debt you keep paying.
- **Detection:** A dep imported in exactly one place, or used for one function. Or: it entered
  without a decision-log line. Or: you can name a <20-line hand-roll that replaces it.
- **Recovery:** Inline the trivial ones and drop the dep; for the rest, justify in writing or cut.
  Let a dead-export gate (knip) catch what's already unused.
- **Prevention:** No new dependency without written justification and approval — make it earn its
  place before it lands (§5). Pin ranges; prefer the simplest thing that holds.
- **Cart Clash / cross-ref:** The stack is deliberately curated and pinned (three, rapier,
  zustand, howler, animejs, tweakpane, nipplejs, partyserver), knip gates unused exports, and TS 7
  was refused because it wasn't worth ~849 JSDoc errors. SIMD Rapier is the case of a dependency-
  path that didn't justify itself (see #7). **§5**.

### 10. Menu Explosion
- **Symptoms:** The menu/settings file becomes a top churn hotspot. Options added faster than
  they're removed. Layout breaks on real screens because there's too much to fit.
- **Causes:** Menus are where "just one more toggle" feels harmless; each option is cheap alone;
  nobody prunes settings nobody changes.
- **Consequences:** A sprawling settings screen, per-DPI/per-resolution layout fragility, and a
  file that churns every time UI is touched.
- **Detection:** The menu file is in the top-3 most-edited files. Or: options-added outpaces
  options-removed over the last N commits. Or: a setting no playtester has ever changed.
- **Recovery:** Cut settings that don't earn their space; pick sane defaults over exposing a knob;
  make the layout adaptive (safe-centering + overflow backstop) so content fits real hardware.
- **Prevention:** A new menu option runs the Player-Value gate like any feature. Default > toggle.
  Judge the layout on real high-DPI hardware, not the dev box.
- **Cart Clash / cross-ref:** Recurring menu churn — DPI-adaptive relayout, uniform columns, and
  a *condensed* How-to-Play (with dead controls-list code deleted); a menu clipped only at 1080p
  @ 125% OS scaling. `cart-rave-menu.js` sits at ~2024 lines. **§6**, **L-06** (real-hardware).

### 11. Singleton Abuse
- **Symptoms:** A global object read — and worse, *written* — everywhere. Runtime code mutating
  shared config. Two overlapping global stores for the same state. Tests need a global reset
  between cases.
- **Causes:** A global is the path of least resistance for "state everyone needs"; mutating it is
  easier than threading a value; a second store appears before the first is retired.
- **Consequences:** Hidden coupling (see #2), order-dependent bugs, un-testable units, and no
  single source of truth for state that two globals both claim.
- **Detection:** Runtime (non-init) code assigns to a global/`CONFIG`. Or: a singleton is imported
  in >N modules. Or: two stores expose the same state. Or: a test fails unless you reset a global
  first.
- **Recovery:** Stop mutating the global — layer runtime modifiers *over* config instead of *into*
  it; collapse duplicate stores to one owner with one import surface.
- **Prevention:** Globals are read-only after init. State that changes at runtime gets an owner
  and an explicit update path, not a mutated singleton.
- **Cart Clash / cross-ref:** **DIR-1** exists to let Living Store directives modify behavior
  *without mutating `CONFIG`* (they currently do — a mutated singleton). **STORE-1** is the
  `gameState` / `gameStore` dual-import-surface collapse. **§4**, and #2 (Hidden State).

### 12. Architecture Astronautics
- **Symptoms:** Abstraction layers, factories, or "generic systems" with exactly one real
  implementation. Building for scale, extensibility, or reuse you can't name a concrete user for.
  Empty scaffolding created "for later."
- **Causes:** Anticipating requirements that never arrive; the pleasure of a clean general
  design; agents over-engineering a simple ask into a framework.
- **Consequences:** Indirection that costs comprehension every day to buy flexibility you never
  use; more surface, slower changes, harder onboarding — the sprawl the OS exists to prevent.
- **Detection:** An interface/factory/plugin-system with one implementation and no second on the
  roadmap. Or: a folder of stub files. Or: you can't name the concrete case the abstraction
  serves *today*.
- **Recovery:** Collapse the abstraction to the concrete case; delete the speculative layer. Add
  generality back the day a *second* real case demands it.
- **Prevention:** Build the simplest thing that holds; earn abstractions from repetition, not
  prophecy. Ask Decision-Framework Q3/Q5/Q8 — cheapest version, simplest rejected option,
  reversibility.
- **Cart Clash / cross-ref:** (general — no shipped instance; the studio catches itself instead:
  RedShiftOS Decision #1 refuses 30 empty stub files *because* that is this anti-pattern, and
  **TRUST-1** — a Worker that validates host-asserted outcomes — is deliberately deferred until a
  leaderboard actually needs it rather than built for imagined cheating.) **§5**, Decision #1.

### 13. Manager-Manager Syndrome
- **Symptoms:** A `Manager` whose main job is coordinating other `Manager`s. Classes that mostly
  forward calls. Three "System/Controller/Manager" layers in one call chain, each thin.
- **Causes:** Naming a thing "Manager" invites a manager for it; coordination logic gets its own
  object instead of living in a pipeline; refactors add a layer instead of removing one.
- **Consequences:** Call chains you trace through four hops to find the one line that does work;
  responsibility smeared so no single owner is accountable.
- **Detection:** Two+ objects named `*Manager`/`*System`/`*Controller` in the same call path, at
  least one of which only delegates. Or: a class with no behavior of its own — pure forwarding.
- **Recovery:** Flatten — inline the pass-through layer, give the real work one owner, and replace
  "a manager coordinates managers" with an explicit ordered pipeline.
- **Prevention:** A coordinator must do coordinating work, not just relay. Prefer one ordered
  pipeline over a hierarchy of managers. One responsibility per file (§4) — including the
  coordinators.
- **Cart Clash / cross-ref:** (general — no instance on record. Cart Clash has managers —
  `levelManager`, `audioManager`, `centerStage` as a one-moment-at-a-time arbiter — but each
  currently owns real single-responsibility work rather than managing other managers. The smell
  to watch as they grow, not a present defect.) **§4**.

---

*New anti-patterns are added the same way Lessons are: when a shape recurs across projects,
name it, make its Detection checkable, and wire the cross-ref. A catalog you can grep is worth
more than one you have to read.*
