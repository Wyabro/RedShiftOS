# Lessons Learned

*Every hard-earned lesson turned into a permanent, reusable rule. This is the most valuable
document in RedShiftOS: it's the difference between making a mistake once and making it
every project. Format is fixed — Problem → Root Cause → Rule → Example — so each entry
teaches the* why*, not just the* what.

> **Seeded from Claude's working memory of the Cart Clash build.** These are real patterns
> observed across the project, but they're one perspective. Wyatt: verify each one, correct
> anything I got wrong, and add the lessons only you remember (the design fights, the
> playtest surprises, the things that felt bad before they were provably wrong).

---

## L-01 — Networking retrofitted onto finished gameplay

- **Problem:** Netcode was introduced after gameplay systems already existed, producing a
  long tail of desync, host-migration, reconciliation, and buffer-limit bugs.
- **Root cause:** Multiplayer assumptions (authority, determinism, serialization size) touch
  every system, but they weren't decided until the systems they touch were already built.
- **Rule:** Cross-cutting concerns — networking, save, input, persistence — must have their
  assumptions documented *before* the gameplay that depends on them is implemented. If a
  game might be multiplayer, decide the authority model on day one, even if MP ships later.
- **Example:** "Server picks the arena" and "client latches room level from hello" were
  fixes for a desync that only existed because arena selection was designed single-player-first.

## L-02 — DEV-only behavior that isn't there in production

- **Problem:** A diagnostic tool (F8 bug capture) silently did nothing in the shipped build,
  even though it worked in dev.
- **Root cause:** The code path was gated on a dev-only build flag, so it compiled *out* of
  the production bundle. The thing you rely on to debug prod wasn't in prod.
- **Rule:** Anything you depend on to diagnose the shipped game must ship *in* the shipped
  game (gated at runtime, not at build time). Assume dev and prod behavior diverge until
  proven identical.
- **Example:** A keydown listener wrapped in a `DEV`-only guard vanished from the prod
  bundle; the fix was a runtime gate, not a compile-time one.

## L-03 — Profiling on hardware that isn't the target

- **Problem:** Performance numbers looked fine in development but the game janked on a
  friend's real machine.
- **Root cause:** Dev profiling ran against a software GPU and dev-server overhead —
  numbers that don't represent a real player's hardware or the production build.
- **Rule:** Judge performance from the *production build* on *representative hardware*
  (including low-end and high-DPI). Never trust dev-environment or software-renderer numbers
  as if they were real.
- **Example:** "Load janks ~30s" was ~18.5s in dev vs ~10.6s in prod; a menu clipped only at
  1080p @ 125% OS scaling — invisible on the dev machine.

## L-04 — Observability built too late

- **Problem:** Whole classes of bugs (netcode stalls, resource leaks, boot-timeline stalls)
  were invisible until a diagnostics framework was built deep into the project.
- **Root cause:** The ability to *see* what the game was doing was treated as a late add-on
  instead of foundational infrastructure.
- **Rule:** Build observability before you need it. A new project gets its capture/diagnostic
  hooks and verification harness early, so problems are measurable the first time they occur.
- **Example:** Once `__ccDiag` (probe registry + event ring buffer) and the headless harness
  existed, previously-mysterious freezes became reproducible and fixable.

## L-05 — "Done" declared without checking the deployed artifact

- **Problem:** Work was believed shipped/fixed when the deployed build didn't actually
  contain or reflect the change.
- **Root cause:** "Done" was measured against local state (it compiles, tests pass) rather
  than against the artifact players actually run.
- **Rule:** Remote is authoritative. A change isn't done until it's verified in the fetched
  production asset — build stamp confirmed, behavior observed live.
- **Example:** Fixes were confirmed only after checking the deployed Version hash against the
  commit and observing the behavior in the fetched bundle.

## L-06 — God-files accumulate responsibility

- **Problem:** Core gameplay logic concentrated into files that grew unwieldy and hard to
  reason about (and dangerous to touch during multiplayer work).
- **Root cause:** Responsibilities accumulated faster than they were extracted; files were
  organized by feature history rather than responsibility.
- **Rule:** Organize by responsibility from the start — movement, collision, boost, cargo as
  separate owners rather than one simulation file. When a file's description needs an "and,"
  split it.
- **Example:** Reentrancy bugs surfaced because scoring, round-ending, and physics-body
  removal all lived close enough to interleave mid-iteration.

## L-07 — Concurrent AI sessions clobber shared files

- **Problem:** Two agent sessions editing the same tree overwrote each other's work
  (main.js, build config), and a shared dev harness got poisoned by concurrent edits.
- **Root cause:** Broad staging (`git add -A`) and long-lived uncommitted work let one
  session's changes stomp another's.
- **Rule:** In an AI-first workflow, commit early and stage surgically — name the files you
  mean to commit, never blanket-add. Treat a co-edited file as a merge point, not a free-for-all.
- **Example:** Recovering co-edited files required `git apply --cached` per-file rather than
  a blanket commit; the discipline is to commit the moment a unit of work is verified.

---

*When a project ends, its postmortem feeds new entries here. The OS gets smarter because we
actually shipped with it.*
