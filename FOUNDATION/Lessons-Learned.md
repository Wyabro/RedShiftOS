# Lessons Learned

*Every hard-earned lesson turned into a permanent, reusable rule. This is the most valuable
document in RedShiftOS: it's the difference between making a mistake once and making it every
project. Format is fixed — Problem → Root Cause → Rule → Example — so each entry teaches the*
why*, not just the* what.

> **Seeded from the Cart Clash build** — reconstructed from four sources: Claude's working
> session memory (production/polish era), the repo's own architecture doc + append-only
> decision log, 800+ commits of git history, and the frozen `main` (jam) branch's founding
> docs (README, `todo.md`) back to the original Cursor Vibe Jam prototype.
> These are real, evidenced patterns, but they're one lens. Wyatt: verify each one, correct
> anything I got wrong, and add the lessons only you remember — the design fights, the
> playtest surprises, the things that felt bad before they were provably wrong.
>
> IDs are stable and append-only from here (like the project's own decision log). When a new
> lesson arrives, give it the next number; don't renumber.

---

## L-01 — A prototype's architecture is not a production architecture

- **Problem:** Cart Clash began as a **Cursor Vibe Jam** prototype ("Initial Cart Rave
  prototype") built to a hard deadline (May 1, 2026) and grew into an 800+‑commit production.
  Much of the early history is raw churn — `chore: diagnose why physics substeps never run`,
  dozens of spawn-ring / collider / center-of-mass tuning commits — and the entire build,
  deploy, and module structure had to be retrofitted onto jam-era bones.
- **Root cause:** A jam optimizes for "shippable before the deadline," not for the
  boundaries a lasting product needs. When it graduated, the prototype was *extended* rather
  than *re-founded* — including infrastructure chosen purely for speed-to-ship.
- **Rule:** When a prototype proves the loop and earns the right to become a product,
  schedule an explicit re-foundation pass — build system, deploy target, module boundaries,
  cross-cutting concerns, verification harness — before piling on features. Assume the jam's
  *infrastructure* gets fully replaced, not just its code. Treat "prototype code" and
  "production code" as different materials, not different amounts of the same code.
- **Example:** The jam prototype had **no bundler** (dependencies via an HTML import map) and
  deployed to **Vercel** at `cartrave.lol`; production retrofitted a **Vite** build, a modular
  `src/` tree, Zustand stores, `bootstrap.js` / `levelManager.js` extractions, and a
  **Cloudflare Workers** deploy. Match length (60s → 150s) and arena count (1 → 3) changed too
  — the game's own core parameters were still being discovered well after "launch."

## L-02 — Cross-cutting concerns: plan early, but expect the hard bugs late

- **Problem:** Multiplayer *intent* was set early, yet the genuinely hard netcode problems —
  host migration, clock-domain mismatches, fall/collision dedupe, reconciliation — surfaced
  much later, and several remain open (live 2‑client smoke is still pending).
- **Root cause:** Planning the concern early is necessary but not sufficient. The transport
  was re-architected mid-project (WebSocket server-relay → WebRTC P2P DataChannels at ~40 Hz),
  and authority/timing edge cases only appear under real multi-client load — which arrives
  long after the feature is "built."
- **Rule:** Decide cross-cutting concerns (authority model, transport, time base) on day one
  *and* budget a dedicated late-stage hardening + live-multi-client verification pass from
  the start. "We planned for multiplayer" does not mean "multiplayer works."
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
- **Root cause:** The code path was gated on a dev-only *build* flag, so it compiled out of
  the production bundle. The thing you rely on to debug prod wasn't in prod.
- **Rule:** Anything you depend on to diagnose the shipped game must ship *in* it, gated at
  runtime, not at build time. Assume dev and prod diverge until proven identical.
- **Example:** A keydown listener wrapped in a `DEV`-only guard vanished from the prod bundle;
  the fix was a runtime gate. (Related: a scary "2s first level-swap" turned out to be a Vite
  dev-transform artifact worth zero production cost — dev noise masquerading as a prod bug.)

## L-06 — Judge performance and look on real target hardware, with levers you can hand off

- **Problem:** Performance and rendering looked fine in development but broke on real
  machines — jank on a friend's PC, a menu clipped only at 1080p @ 125% OS scaling, and a
  black-frame flicker that never once reproduced in the offline test rig.
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
- **Root cause:** A performance optimization was made the *default* before it was proven safe
  under the real game, with no cheap fallback.
- **Rule:** Don't optimize before playtests point at the need, and never make an unproven
  optimization the default — gate it opt-in behind a working fallback until it earns trust.
  Every optimization is a dependency that must justify itself and degrade gracefully.
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
- **Example:** Recovering co-edited files required `git apply --cached` per file rather than
  a blanket commit — the fix was discipline, not tooling.

---

*When a project ends, its postmortem feeds new entries here. The OS gets smarter because we
actually shipped with it.*
