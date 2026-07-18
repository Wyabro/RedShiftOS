# Lessons Learned

*Every hard-earned lesson turned into a permanent, reusable rule. This is the most valuable
document in RedShiftOS: it's the difference between making a mistake once and making it every
project. Format — Problem → *(Why we missed it)* → Root Cause → Rule → *(Exceptions)* →
Example — so each entry teaches the* why*, not just the* what*. The parenthetical fields
appear where they add signal:* **Why we missed it** *(the blind spot that let it happen —
this is what teaches judgment) and* **Exceptions** *(when the rule does* not *apply, so it
doesn't get cargo-culted).

> Lessons are earned from real shipped work, not theory. IDs are **append-only and stable** —
> a new lesson gets the next number; never renumber or silently delete. Add the head-only ones
> as you build: the design fights, the playtest surprises, the things that felt bad before they
> were provably wrong.

## Pull by tag

Load the lessons a task needs, not all seventeen:

- **architecture** — L-01, L-03, L-04, L-17
- **net / multiplayer** — L-02, L-08
- **ship / verify** — L-05, L-07, L-08
- **perf / hardware** — L-06, L-09, L-10
- **process / workflow** — L-01, L-10, L-11, L-12, L-14, L-17
- **ai-collaboration** — L-05, L-11, L-13, L-14
- **design / feel** — L-13, L-16
- **playtest** — L-06, L-15, L-16

---

## L-01 — A prototype's architecture is not a production architecture

- **Problem:** Cart Clash began as a **Cursor Vibe Jam** prototype ("Initial Cart Rave
  prototype") built to a hard deadline (May 1, 2026) and grew into an 800+‑commit production.
  Much of the early history is raw churn — `chore: diagnose why physics substeps never run`,
  dozens of spawn-ring / collider / center-of-mass tuning commits — and the entire build,
  deploy, and module structure had to be retrofitted onto jam-era bones.
- **Why we missed it:** It was a jam. There was no time or reason to build production
  boundaries, and a working prototype *feels* like a foundation — nothing signals "this
  scaffolding is disposable" until you're already standing on it.
- **Root cause:** A jam optimizes for "shippable before the deadline," not for the
  boundaries a lasting product needs. When it graduated, the prototype was *extended* rather
  than *re-founded* — including infrastructure chosen purely for speed-to-ship.
- **Rule:** When a prototype proves the loop and earns the right to become a product,
  schedule an explicit re-foundation pass — build system, deploy target, module boundaries,
  cross-cutting concerns, verification harness — before piling on features. Assume the jam's
  *infrastructure* gets fully replaced, not just its code. Treat "prototype code" and
  "production code" as different materials, not different amounts of the same code.
- **Exceptions:** A true throwaway you'll never ship needs no re-foundation. The trap is only
  when a prototype *graduates* — that's the moment to re-found, not extend.
- **Example:** The jam prototype had **no bundler** (dependencies via an HTML import map) and
  deployed to **Vercel** at `cartrave.lol`; production retrofitted a **Vite** build, a modular
  `src/` tree, Zustand stores, `bootstrap.js` / `levelManager.js` extractions, and a
  **Cloudflare Workers** deploy. Match length (60s → 150s) and arena count (1 → 3) changed too
  — the game's own core parameters were still being discovered well after "launch."

## L-02 — Cross-cutting concerns: plan early, but expect the hard bugs late

- **Problem:** Multiplayer *intent* was set early, yet the genuinely hard netcode problems —
  host migration, clock-domain mismatches, fall/collision dedupe, reconciliation — surfaced
  much later, and several remain open (live 2‑client smoke is still pending).
- **Why we missed it:** Single-client testing looked completely fine. The hard authority and
  timing bugs are invisible until several real machines hit the system at once — which happens
  long after the feature already reads as "done."
- **Root cause:** Planning the concern early is necessary but not sufficient. The transport
  was re-architected mid-project (WebSocket server-relay → WebRTC P2P DataChannels at ~40 Hz),
  and authority/timing edge cases only appear under real multi-client load — which arrives
  long after the feature is "built."
- **Rule:** Decide cross-cutting concerns (authority model, transport, time base) on day one
  *and* budget a dedicated late-stage hardening + live-multi-client verification pass from
  the start. "We planned for multiplayer" does not mean "multiplayer works."
- **Exceptions:** A genuinely single-player game can defer this — but decide it's
  single-player *on purpose*, don't back into it because multiplayer felt hard.
- **Example:** The jam shipped host-authoritative sync as **60 Hz input / 20 Hz transforms
  over the PartyKit WebSocket relay**; production re-architected to **~40 Hz binary snapshots
  over WebRTC P2P DataChannels** — a transport swap, not a tweak. The old "server forwards
  inputs" note went stale, and migration ghost-freeze / dedupe hazards tracked in
  `netcode-deep-dive.md` remain partly unverified.

## L-03 — Module boundaries are load-bearing

- **Problem:** The menu and gameplay can't be code-split, so a player sitting on the menu
  downloads the entire game engine. An obvious win is blocked.
- **Root cause:** Menu-coupled files statically import gameplay modules (the netcode module is
  referenced in 132 places, including at the menu; the HUD runs per-frame). There's no clean
  seam to split on — coupling that was cheap to allow early hardened into a wall.
- **Rule:** Decide early which clusters must stay separable (menu vs. gameplay, sim vs.
  render) and enforce the seam continuously. Coupling is cheap to add and expensive to
  remove; a boundary is the opposite.
- **Example:** "BUNDLE-1" is blocked pending a full gameplay-cluster-behind-one-dynamic-import
  refactor that should have been a boundary from the first week.

## L-04 — Every system owns one responsibility

- **Problem:** Core gameplay logic concentrated into files that grew hard to reason about and
  dangerous to touch during multiplayer work.
- **Root cause:** Responsibilities accumulated faster than they were extracted; files were
  organized by feature history rather than by responsibility.
- **Rule:** Organize by responsibility from the start — movement, collision, boost, cargo as
  separate owners rather than one simulation file. When a file's description needs an "and,"
  split it.
- **Example:** Reentrancy bugs surfaced because scoring, round-ending, and physics-body
  removal lived close enough to interleave mid-iteration.

## L-05 — Dev behavior is not production behavior (beware build-time gates)

- **Problem:** A diagnostic tool (F8 bug capture) silently did nothing in the shipped build,
  even though it worked in dev.
- **Why we missed it:** You only ever run the dev build while building. A build-time gate
  looks harmless in the editor and gives zero signal that it compiled *out* of the thing
  players actually run.
- **Root cause:** The code path was gated on a dev-only *build* flag, so it compiled out of
  the production bundle. The thing you rely on to debug prod wasn't in prod.
- **Rule:** Anything you depend on to diagnose the shipped game must ship *in* it, gated at
  runtime, not at build time. Assume dev and prod diverge until proven identical.
- **Exceptions:** Instrumentation you'd truly never want in production can stay build-gated —
  just never build-gate the things you diagnose *prod* with.
- **Example:** A keydown listener wrapped in a `DEV`-only guard vanished from the prod bundle;
  the fix was a runtime gate. (Related: a scary "2s first level-swap" turned out to be a Vite
  dev-transform artifact worth zero production cost — dev noise masquerading as a prod bug.)

## L-06 — Judge performance and look on real target hardware, with levers you can hand off

- **Problem:** Performance and rendering looked fine in development but broke on real
  machines — jank on a friend's PC, a menu clipped only at 1080p @ 125% OS scaling, and a
  black-frame flicker that never once reproduced in the offline test rig.
- **Why we missed it:** The dev machine physically *cannot* reproduce a driver-specific quirk,
  so from where you're standing the bug simply doesn't exist. Absence in dev is not evidence of
  absence.
- **Root cause:** Dev ran a software GPU (SwiftShader) and dev-server overhead — which
  literally *cannot* reproduce driver-specific quirks like the ANGLE/NVIDIA half-float bug.
  Truth lives on the target stack, not the dev box.
- **Rule:** Judge performance and visuals from the *production build* on *representative
  hardware* (including low-end and high-DPI). For bugs that only exist on hardware you don't
  have, build A/B *levers* you can hand to an affected playtester (`?rtmode=`, `?blackmon=`)
  and diagnose on their machine — don't try to repro locally what your hardware can't produce.
- **Example:** The flicker was root-caused only when Wyatt ran `?rtmode=` toggles on real
  NVIDIA/ANGLE hardware: default vs. `bloombyte` differed *only* in bloom-mip type → the
  culprit was the half-res float bloom mips, provable no other way.

## L-07 — "Done" means verified in the artifact you actually ship

- **Problem:** Work was believed shipped/fixed when the deployed build didn't contain or
  reflect the change.
- **Root cause:** "Done" was measured against local state (it compiles, tests pass) rather
  than the artifact players run.
- **Rule:** Remote is authoritative. A change isn't done until it's verified in the fetched
  production asset — build stamp confirmed, behavior observed live, cache-busted so the CDN
  can't mask it.
- **Example:** Fixes were confirmed only after matching the deployed Version hash to the
  commit and observing the behavior in the fetched bundle.

## L-08 — Know your platform's state and liveness contract before you lean on it

- **Problem:** Production kept stale room/lobby state after deploys, and "the connection is
  live" proved unreliable (ghost hosts, zombie sockets, tabs vanishing uncleanly).
- **Root cause:** The platform's lifecycle contract wasn't treated as a design input.
  Cloudflare Durable Object in-memory state persists across deploys (deploy ≠ reset), and a
  platform "connected" status is not a liveness guarantee.
- **Rule:** Learn the platform's state and liveness model before depending on it. Assume a
  deploy does not reset server state, and treat "live" as a hypothesis you confirm with your
  own heartbeats/reaper — not a fact the platform hands you.
- **Example:** A heartbeat-based reaper (last-activity per connection + stale removal) was
  required so host handoff could proceed from a repaired live-set; DO state needs manual
  eviction to clear.

## L-09 — Optimize only after proof; keep unproven optimizations opt-in

- **Problem:** An attractive physics optimization (SIMD Rapier) shipped a game-breaking
  borrow error and had to be reverted.
- **Why we missed it:** The optimization was real and it worked in the first cases tried;
  "make it the default" felt like shipping a win, not betting on unproven code under the whole
  game.
- **Root cause:** A performance optimization was made the *default* before it was proven safe
  under the real game, with no cheap fallback.
- **Rule:** Don't optimize before playtests point at the need, and never make an unproven
  optimization the default — gate it opt-in behind a working fallback until it earns trust.
  Every optimization is a dependency that must justify itself and degrade gracefully.
- **Exceptions:** A measured, isolated, proven win with a working fallback is fine to adopt —
  the rule targets *unproven* optimizations made default, not optimization itself.
- **Example:** `9d8a69e` preferred SIMD Rapier; `8174180` reverted to standard Rapier by
  default after the borrow error — SIMD stayed opt-in only.

## L-10 — Build observability before you need it

- **Problem:** Whole classes of bugs (netcode stalls, resource leaks, boot-timeline stalls)
  were invisible until a diagnostics framework was built deep into the project.
- **Root cause:** The ability to *see* what the game was doing was treated as a late add-on
  instead of foundational infrastructure.
- **Rule:** Build observability early — capture hooks, a probe/event ring buffer, a headless
  verification harness — so problems are measurable the first time they occur, not the tenth.
- **Example:** Once `__ccDiag` (probe registry + event ring buffer) and the headless harness
  existed, previously-mysterious freezes became reproducible and fixable in one pass.

## L-11 — In an AI-first workflow, commit early and stage surgically

- **Problem:** Two agent sessions editing the same tree overwrote each other's work
  (`main.js`, build config), and a shared dev harness was poisoned by concurrent edits.
- **Root cause:** Broad staging (`git add -A`) and long-lived uncommitted work let one
  session's changes stomp another's.
- **Rule:** Commit the moment a unit of work is verified, and stage surgically — name the
  files you mean to commit, never blanket-add. Treat a co-edited file as a merge point.
  Checkpoint-commit *before* any risky agent run, and keep agent work on a feature branch — so
  if it makes a mess you discard the branch instead of debugging in place. Git is your primary
  safety net against agent mistakes, not just version control.
- **Example:** Recovering co-edited files required `git apply --cached` per file rather than
  a blanket commit — the fix was discipline, not tooling.

## L-12 — Seam regressions live between the fixes, not in them

- **Problem:** A sprint of individually-correct fixes shipped a fresh crop of bugs — none in
  any single fix, all in the seams between them (fall-queue lifecycle, clock domains, override
  slots, phase-flip ordering).
- **Root cause:** Each fix was verified in isolation; nobody reviewed how the fixes
  *interacted*. Bugs breed in the interfaces, not the commits.
- **Rule:** After any batch of fixes, run a dedicated integration pass that targets
  interactions — shared lifecycles, ordering, state that multiple fixes touch — not the
  individual diffs. Assume the next bug is in a seam.
- **Example:** A post-sprint review found 9 regressions, every one in a seam between fixes (a
  solo fall-queue backlog spawning phantom KOs; a skewed host clock that could never end a
  round) — zero in the fixes themselves.

## L-13 — Systems only know what you tell them about the world

- **Problem:** NPC carts drove straight into one arena's center hole and self-destructed
  constantly, while the same bots handled every other arena's hazards fine.
- **Root cause:** That arena's center hole had no AI path routing, so bots drew straight-line
  chords across it. A hazard existing in the world is not a hazard the AI knows about —
  awareness is per-hazard, per-arena, and explicit.
- **Rule:** Wire up explicit AI/system awareness for every hazard in every arena. When you add
  a hazard, add its routing/avoidance in the same breath — don't assume existing logic
  generalizes to it.
- **Example:** Adding hole-routing waypoints + a time-to-lip panic brake cut bot self-KOs
  from ~63 in a soak to 5 per 150 s round, zero into the hole.

## L-14 — Root-cause before the obvious fix, and put a number on it

- **Problem:** The intuitive fix for suiciding bots was "make the carts turn easier" — which
  would have masked the real cause and dulled the driving feel for everyone.
- **Root cause:** The obvious lever treated a symptom (bots can't avoid the hole) as the cause
  (turning too stiff). The real cause was missing path routing; physics was never the problem.
- **Rule:** Find the real cause before reaching for the intuitive fix — the obvious knob often
  papers over the bug and costs feel elsewhere. Treat what you observe as ground truth: if the
  bug is still visible, it's still real — don't let an agent explain it away. Force the frame —
  three candidate causes, the smallest test for each, what you'd see if each were true — and
  agree the diagnosis before the fix. Measure it (a soak, a metric) before and after, so
  "fixed" is proven, not hoped.
- **Example:** Instead of loosening handling, the fix added hole routing; a soak proved it
  (~63 → 5 self-KOs) without touching how the carts drive.

## L-15 — Playtest the first-time experience with your shortcuts off

- **Problem:** The real progression / onboarding funnel had never actually been played — the
  dev profile unlocks everything, so nobody had seen what a brand-new player sees.
- **Root cause:** Development accretes dev flags, unlocks, and deep-link shortcuts that quietly
  hide the new-player experience. You test the game you can reach, not the one they land in.
- **Rule:** Playtest the FTUE on a fresh profile with dev shortcuts OFF. And sequence
  playtests deliberately: drain your own taste-tuning queue first (tuning invalidates later
  feel notes), and save fresh external testers for one-shot first impressions — never spend
  them on known bugs.
- **Example:** Cart Clash gated a real onboarding session behind a `devUnlocks=off` clean
  profile precisely because "dev unlocks all" had masked the funnel the whole project.

## L-16 — Separate "intense" from "broken"

- **Problem:** Aggressive systems (rubberband catch-up, every bot converging on one target)
  read as "too much" — but toning them down would have sanded off the exact chaos the game is
  built on.
- **Why we missed it:** Playtesters (and you) report "that felt like too much" the same way
  for spice and for defects — the *feeling* is identical; only the cause differs.
- **Root cause:** "Feels intense" and "is broken" get lumped together in playtest reactions.
  Only one of them is a defect.
- **Rule:** When something feels harsh, decide first whether it's a bug or the spice. Fix the
  bugs; keep the intensity that's on-theme. Don't smooth the game's edges just because they're
  sharp.
- **Exceptions:** Intensity that actually drives players away — confusion, unfairness, motion
  sickness — is a real defect, not spice. "Hard and chaotic" is spice; "I want to stop
  playing" is not.
- **Example:** The rubberband + single-target convergence were kept by explicit choice — the
  bugs in them were fixed, the intensity left intact.

## L-17 — When a bug class keeps coming back, enforce an invariant

- **Problem:** "Every audio change breaks something" was a recurring class — menu music playing
  over game music, mute double-toggling — reappearing every time the audio code was touched.
- **Root cause:** The rule (one music track at a time; one input handler) lived as discipline
  spread across call sites, so any new code could quietly violate it.
- **Rule:** When the same class of bug keeps recurring, stop fixing instances — move the rule
  into one place as an enforced invariant with a regression test. Make the wrong thing
  impossible, not just currently-absent.
- **Example:** Music exclusivity was moved *inside* the audio manager (starting game music
  stops menu music, and vice-versa) with regression tests, structurally closing the class; a
  duplicate keydown listener that double-fired mute was collapsed to one.

---

*When a project ends, its postmortem feeds new entries here. The OS gets smarter because we
actually shipped with it.*
