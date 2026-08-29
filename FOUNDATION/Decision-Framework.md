# Decision Framework

*The checklist you run before committing to any major design or technical decision, plus the
format for recording it. A feature that can't answer these questions probably isn't ready.
Six months from now, when you wonder "why did I do this?", the log is the answer — and it
gives AI agents the* reasoning *behind the architecture, not just the architecture.*

---

## Where decisions live

This log is for **OS-level** decisions (structure, rules, process). **Game-specific** decisions
and burns live in the game's own repo (a `decisions.md` / postmortem) — keeping them there
avoids coupling the RedShiftOS submodule pin to one game's history. Promote a decision into this
log only when it's about the OS itself, or when a *second* game re-derives the same lesson.

## The pre-decision checklist

Before committing to a major decision, answer:

1. **Why are we doing this?** What problem or opportunity, in one sentence.
2. **Which design pillar does this support?** If none, why does it exist?
3. **Can this be prototyped first?** What's the cheapest version that tests the assumption?
4. **What existing systems change?** Especially cross-cutting concerns (net, save, input).
5. **Is there a simpler solution?** Name the simplest option you're rejecting, and why.
6. **Will this increase maintenance cost?** New dependencies, new abstractions, new surface area.
7. **How will we know if it worked?** The measurable signal, defined *before* you build.
8. **Can we remove it later?** Is this reversible, or are we locking the design in?

If a decision can't answer 1, 2, and 7, it's not a decision yet — it's a hunch.

## Decision Log format

Every significant decision gets an entry. Keep them append-only; when a decision is
reversed, add a *new* entry that supersedes the old one (don't rewrite history).

```
## Decision #N — <short title>
Date: YYYY-MM-DD
Status: Proposed | Accepted | Superseded by #M

Question:
  <the decision to be made>

Alternatives:
  - <option A>
  - <option B>
  - <option C>

Chosen:
  <the option>

Reason:
  <why — tied to a pillar or a lesson>

Tradeoffs:
  <what we give up>

Reversible?:
  <yes/no; if no, what locks it in>
```

---

## Decision Log

## Decision #1 — RedShiftOS v0.1 scope: foundation only, no stub files

Date: 2026-07-17
Status: Accepted

Question:
  What goes in the first version of RedShiftOS — the full multi-folder document tree, or a
  minimal foundation?

Alternatives:
  - Full tree now: ~30 documents across FOUNDATION / DESIGN / ENGINEERING / AI / PRODUCTION,
    created as stubs to be filled in later.
  - Foundation only: README + four cornerstone documents, no empty folders or stub files.

Chosen:
  Foundation only — README, Development Manifesto, Lessons Learned, Decision Framework,
  Feature Lifecycle.

Reason:
  The OS exists to prevent organic sprawl (the core Cart Clash lesson). Creating 30 empty
  stubs *is* organic sprawl — it recreates the exact anti-pattern the manifesto warns
  against. A document should exist only when it holds real content. Simplicity beats
  cleverness (Manifesto §7).

Tradeoffs:
  The eventual structure (DESIGN, ENGINEERING, AI, PRODUCTION, TEMPLATES, GAME_TEMPLATE) is
  not visible up front, so the shape has to be discovered as content earns its place.

Reversible?:
  Yes — folders and documents are added at any time when they have real content to hold.

## Decision #2 — Agent wiring: how games consume the OS

Date: 2026-07-17
Status: Accepted — load order superseded in part by #5; pointer set extended by #8;
  supported layout superseded in part by #9

Question:
  How do agents get pointed at RedShiftOS, and where does the wiring live?

Alternatives:
  - Hand-roll each game's rules from scratch (the ai-builder-playbook era — it drifted and
    the current version got "botched").
  - A canonical `AGENTS.md` in the OS + a copy-in `GAME_TEMPLATE/` every game repo uses.

Chosen:
  Root `AGENTS.md` (canonical, read-first) + thin `CLAUDE.md` pointer + `GAME_TEMPLATE/`
  (`AGENTS.md` + `PROJECT.md` + `README.md`) that a new game copies. RedShiftOS rides beside
  each game as a git submodule or sibling checkout. Session load order = game `AGENTS.md` →
  OS `FOUNDATION` → OS `PROCESS` → game `PROJECT.md` → tasks / latest handoff.

Reason:
  Mirrors what already works in cart-rave (`AGENTS.md` canonical, `CLAUDE.md` pointer) and
  Wyatt's own ai-builder-playbook starter-template. Rules live in the repo and every session
  reads them first; the OS is the shared context that stops per-game drift.

Tradeoffs:
  Each game must keep RedShiftOS in sync (submodule pin vs. sibling checkout). `GAME_TEMPLATE`
  is real, usable content, so it earns its folder under Decision #1's rule.

Reversible?:
  Yes — the wiring is copy-in; templates evolve as real games reveal what they need.

## Decision #3 — Fold the best of ai-builder-playbook into the OS

Date: 2026-07-17
Status: Accepted

Question:
  Wyatt's earlier `ai-builder-playbook` repo (main / v5 / v6 snapshots) holds a lot of
  hard-won AI-workflow guidance, but it drifted and the current version is "botched." What
  gets imported into RedShiftOS?

Alternatives:
  - Leave it in the playbook repo and reference it.
  - Fold the transferable nuggets into RedShiftOS, routed to the right docs.

Chosen:
  Fold in all four reviewed bundles, routed by fit (not dumped in one place):
  - **Verification** → Feature-Lifecycle (dual-gate + proof ladder) and Lesson L-14
    (visible-bug ground-truth).
  - **Agent working-loop** → AGENTS.md (the loop + writer-reviewer + single-top-task), Lesson
    L-11 (checkpoint commits), `PROCESS/Session-Handoffs.md`, `GAME_TEMPLATE/Tasks.md`.
  - **AI layer** → new `AI/` folder: `Model-Routing.md` (+ velocity debt),
    `Triage-and-Recovery.md`, `Prompt-Library.md`.
  - **Re-imported depth v6 dropped** → `REFERENCE/Assets-and-Provenance.md` + Feature-Lifecycle
    (say-the-feeling, kill-a-prototype).

Reason:
  It's Wyatt's own proven material and the playbook was decaying; RedShiftOS is the durable
  home. Routing by fit (vs. one dumping-ground doc) keeps each doc single-responsibility and
  the always-read rules files dense.

Tradeoffs:
  Some playbook depth was deliberately left behind for now (full stack-selection tables, the
  complete prompt library, publishing / OWASP-LLM safety) — captured in session notes, to
  import if a game actually needs it.

Reversible?:
  Yes — docs can be trimmed or merged as real games show what's actually used.

## Decision #4 — Reorganize around the development lifecycle

Date: 2026-07-17
Status: Accepted — always-load model & empty phase folders superseded in part by #5

Question:
  A review (Wyatt + a friend) proposed organizing the OS around *time* — the phases of building
  a game — instead of around document type, plus adding a Philosophy doc, an Anti-Patterns
  catalog, and richer Lessons. How much, and how?

Alternatives:
  - Keep the document-organized layout (FOUNDATION / PROCESS / AI / REFERENCE).
  - Full chronological reorg into phase folders now.
  - Adopt the order as navigation only; defer the folders.

Chosen:
  Full reorg. FOUNDATION stays (how we think — always loaded) and gains `Studio-Philosophy.md`,
  `Anti-Patterns.md`, and `Feature-Lifecycle.md` (moved from PROCESS). A new `LIFECYCLE/` holds
  the seven chronological phases (`1-PROJECT` … `7-POSTMORTEMS`), each with a real index README;
  `Session-Handoffs.md` → `LIFECYCLE/4-IMPLEMENTATION/`, `Assets-and-Provenance.md` →
  `LIFECYCLE/6-PRODUCTION/`. `AI/` and `GAME_TEMPLATE/` stay as cross-cutting layers; `PROCESS/`
  and `REFERENCE/` dissolved. Also: one values doc (Philosophy folds in the non-duplicate
  decision lenses — no separate `Principles.md`); Lessons gained "Why we missed it" +
  "Exceptions" fields where they add signal; studio name standardized to "Red Shift Studios."

Reason:
  The OS should walk you through development in order (the review's insight), and the project
  infographic's banded model (foundation pillars / lifecycle / AI layer) maps onto it. Phase
  folders carry real index content, so they're navigation — not the empty stubs Decision #1
  forbids. A `LIFECYCLE/` wrapper (vs. eight flat top-level folders) keeps FOUNDATION stable
  and the phases correctly ordered.

Tradeoffs:
  Paths recorded in Decisions #2/#3 (`PROCESS/Session-Handoffs.md`,
  `REFERENCE/Assets-and-Provenance.md`) are now stale — those entries are historical record
  (append-only). Current homes: `FOUNDATION/Feature-Lifecycle.md`,
  `LIFECYCLE/4-IMPLEMENTATION/Session-Handoffs.md`, `LIFECYCLE/6-PRODUCTION/Assets-and-Provenance.md`.
  Several LIFECYCLE phase folders are README-only until real games fill them.

Reversible?:
  Yes — folders and moves are cheap; the structure evolves as real games reveal what each
  phase actually needs.

## Decision #5 — Thin the boot load; freeze pending Game #2

Date: 2026-07-17
Status: Accepted — freeze lifted in part by #6

Question:
  A critical external review argued the always-load design (~16k tokens of FOUNDATION every
  session) fights how agents use context, the dual catalog duplicates itself, the pipeline is
  too absolute, and the OS is unproven without a second game. What changes?

Alternatives:
  - Defend and keep as-is.
  - Throw it out (partly rhetorical overreach — the lessons are earned from real shipping, not
    conjecture).
  - Corrective pass: cut the delivery-mechanism overhead, protect the earned content, then freeze.

Chosen:
  The corrective pass. The distinction that drove it: the *lessons* are proven (a real
  jam → production arc that shipped); the *delivery mechanism* is not. So — protect every
  lesson, the philosophy, the proof ladder, the prompts; cut the force-feeding:
  - `AGENTS.md` → thin always-load card (Manifesto + a task map); FOUNDATION pulled by task.
  - `Feature-Lifecycle` → task classes (new-system / feature / bug / tune / chore); killed
    "no skipping, no exceptions"; postmortems are event-driven, not per-feature.
  - `Anti-Patterns` → grep-able detection checklist (was a 13-section essay duplicating Lessons).
  - `Lessons-Learned` → "Pull by tag" index; stayed single-source (an index + full split would
    be the L-17 two-copy sync trap the reviewer's own fix walked into).
  - Removed the empty LIFECYCLE phase folders; game decisions live in the game repo.
  - Defined Game-#2 success metrics (ROADMAP) and froze OS expansion until they move.

Reason:
  A fair test of the hypothesis needs a thinned OS — a bloated always-load would just confirm
  "agents ignore it," which the review already predicted. The material is a real asset; the
  packaging was over-built. Where the review overreached (lessons = conjecture; split Lessons
  into two files) we didn't follow.

Tradeoffs:
  Depth moved from always-on to on-demand — agents must actually *pull* it (the task map is the
  bet). The lifecycle-as-navigation is looser now.

Reversible?:
  Yes — and Game #2 is the real reversibility test: it decides what comes back, what stays cut,
  and what finally earns automation.

## Decision #6 — Lift the Game #2 freeze for an evidence kernel (v0.2)

Date: 2026-08-28
Status: Accepted

Question:
  Game #2 (Slop Park) used RedShiftOS and dead-ended: agents stamped PASS, Wyatt’s verdict is
  FAIL. Decision #5 froze OS expansion until Game #2 produced evidence. What do we add?

Alternatives:
  - Keep the freeze (treat Slop Park as “not a real test”).
  - Import Cart Clash tooling wholesale (BRIEFING generator, ARCHITECTURE.json, npm qa).
  - Import a 60+ Godot skill pack and a large MCP into this repo.
  - Small evidence kernel: states, one-question prototypes, Godot 4.7 cuts, ranked agent loop,
    skill *allow-list* in the game repo only.

Chosen:
  The evidence kernel (v0.2 docs). Slop Park is **frozen** — do not polish it. Primary job of
  v0.2 is to **reject weak hooks quickly**; Cart Clash production discipline applies after KEEP.

  In scope: Feature-Lifecycle evidence states; L-18–L-22; Anti-Patterns 14–18; `AI/Agent-Loops.md`;
  `LIFECYCLE/3-ENGINEERING/Godot.md` (Godot **4.7.x**, stock Jolt, `godot-mcp-go` as an eval
  candidate, awesome-gamedev-agent-skills **4.7** allow-list); GAME_TEMPLATE prove-then-structure;
  Steam provenance note.

  Out of scope: automation wizard, vendoring MCP/skills into RedShiftOS, gag pick for Game #3,
  BRIEFING generators.

Reason:
  Decision #5 asked Game #2 to move the metrics. It moved them the wrong way: false PASS,
  scope stacked, experimental physics, critic theater. Cart Clash’s later gates (playtest debt,
  Wyatt-only PASS) matured after the freeze and are more valuable than another rulebook.
  Godot 4.7 commercial proof exists; the hole was operational gates, not the engine.

Tradeoffs:
  Always-load stays two files (task map grew by two rows). Godot-only engineering page — other
  engines wait. MCP is named, not bundled; it must pass `doctor` on this Windows box before a
  game pins it.

Reversible?:
  Yes — docs can be cut if Game #3 shows they are lore. Automation still waits for a game past
  KEEP to name a repetitive step.

## Decision #7 — Gate the transition from kept prototype to production

Date: 2026-08-28
Status: Accepted

Question:
  How does a game avoid both Cart Clash’s late whole-product refactor and Slop Park’s
  production ceremony before a fun verb exists?

Alternatives:
  - Design production architecture before the concept prove.
  - Extend a kept prototype and schedule a broad refactor when its limits appear.
  - After HUMAN KEEP, stop feature work for one small, explicit Production Foundation Gate.

Chosen:
  A concept prove stays disposable and architecture-light. HUMAN KEEP opens a one-time
  Production Foundation Gate; it does not open production features. The gate requires a
  game-local ownership contract, decisions for applicable cross-cutting concerns, a working
  production skeleton, one representative change drill, and named headless/export evidence.
  An agent may report FOUNDATION TECH PASS. Only Wyatt writes FOUNDATION APPROVED, and
  production features remain blocked until then.

  Run the gate again only if a foundational lock changes: engine, target platform,
  multiplayer authority, save model, or an equivalent project-wide boundary.

Reason:
  Cart Clash extended jam infrastructure until coupling made later separation expensive.
  Slop Park proved the opposite failure: architecture work before a kept verb is theater.
  The cheapest safe point to establish lasting boundaries is immediately after KEEP, while
  the game is proven but still small.

Tradeoffs:
  Every kept concept pays for one deliberate rewrite and a short pause before content grows.
  The ownership contract needs maintenance as real evidence changes it. The gate reduces
  refactor blast radius; it cannot predict every later requirement and is not an architecture
  freeze. It adds no framework, generator, dependency, or pre-KEEP ceremony.

Reversible?:
  Yes — the rule and template fields can be removed if Game #3 shows no improvement. A
  game’s approved contract remains game-local and can evolve through superseding decisions.

## Decision #8 — Thin pointers for every auto-read filename (agent-agnostic boot)

Date: 2026-08-28
Status: Accepted

Question:
  Wyatt switches models often. How does a game (and this OS) stay on one rule set without
  each tool growing its own copy of the process?

Alternatives:
  - Keep a single `CLAUDE.md` pointer and hope other tools find `AGENTS.md`.
  - Import Cart Clash’s generated BRIEFING, ARCHITECTURE.json, and git-hook machine.
  - Canonical `AGENTS.md` plus thin pointers for each well-known auto-read filename.
    Pointers never restate stack, gates, or invariants. A paste-able opener covers tools
    that do not auto-read. Each game names its own primaries in `PROJECT.md`.

Chosen:
  The thin-pointer set. OS and `GAME_TEMPLATE/` ship `GROK.md`, `CLAUDE.md`, `GEMINI.md`,
  and `.cursorrules`. Codex reads `AGENTS.md` natively. If another tool auto-reads a
  different filename, add a thin pointer — do not restate the rules. Do not freeze Cart
  Clash’s 2026-08 routing table (Grok + Codex, Claude demoted) as studio law.

  Still out of scope (Decision #6): BRIEFING generators, ARCHITECTURE.json, Claude hooks as
  process authority, Routine/Standard/Critical lanes, `skills:sync`.

Reason:
  Cart Clash lost a day to copies that drifted (`CLAUDE.md` restated gates; they rotted).
  The fix that survived was one canonical file plus thin pointers. Game #3 needs that on
  day 0, not after KEEP. The production machine (BRIEFING, qa hooks) waits until a game
  past FOUNDATION APPROVED names a repetitive step.

Tradeoffs:
  Four extra files in the template. They must stay thin or they become the problem they
  solve. A new vendor filename is a copy-in, not a redesign.

Reversible?:
  Yes — delete unused pointers. `AGENTS.md` remains the source.

## Decision #9 — Supported layout, record triggers, and v0.2 as a field trial

Date: 2026-08-28
Status: Accepted

Question:
  How does a new game attach RedShiftOS, when do required game-local records exist, and
  what status may we claim for v0.2 before Game #3 produces outcome evidence?

Alternatives:
  - Keep advertising submodule or sibling checkout (current). Sibling paths do not work:
    every pointer and opener uses `RedShiftOS/...`.
  - Support both layouts now by dual-path wording (`../RedShiftOS/...` for sibling).
  - Submodule at `./RedShiftOS` only; create required records at trigger; call v0.2 a
    Game #3 field trial, not a proven full-cycle OS. Pin the submodule; change the pin
    only through a named game decision plus a cold-start smoke test.
  - Delay Game #3 until a doctor, schemas, or bootstrap wizard exist.

Chosen:
  Submodule only + trigger-created records + field-trial status.

  Supported layout is a git submodule at `./RedShiftOS`, pinned to a named commit.
  Sibling checkout is not supported until a later decision adds resolved paths.

  Do not seed empty `decisions.md`, provenance, handoff, or export files at copy-in
  (Decision #1). Create each at its trigger:
  - `decisions.md` — first significant game decision
  - provenance tables — first external or AI-assisted asset
  - latest handoff — end of the first session
  - export / release notes — after FOUNDATION APPROVED, on the first real export

  v0.2 is field-ready for Game #3 concept proof and the post-KEEP Production Foundation
  Gate. It is not production-proven. Measure it with the ROADMAP v0.2 metrics. Do not
  add a doctor, schema, or wizard until a game past KEEP names a repetitive step
  (Decision #6).

Reason:
  A 2026-08-28 readiness review found the sibling claim false, the required records
  described but unseeded, and no field result for v0.2. Advertising a layout that
  does not work is documentation theater. Empty seeds are the stub sprawl Decision #1
  forbids. Game #2 dead-ended under v0.1; Game #3 is the test of v0.2 (Decision #6).
  Calling the OS proven before that trial would be the defect.

Tradeoffs:
  A sibling checkout needs a later path-resolution change. Record creation stays
  manual — Wyatt still has to notice a missed trigger. No upgrade guide yet; the pin
  plus a named decision is the upgrade process.

Reversible?:
  Yes — a later decision can add sibling path resolution, a tiny health check, or
  stronger versioning after Game #3 exposes repeated manual failures.
