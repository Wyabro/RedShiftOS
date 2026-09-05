# Feature Lifecycle

*The full path a **new system** follows, idea to merged — the development heartbeat. Smaller
work runs a subset (see Task classes below). Each stage has one narrow objective, which is
what keeps an agent from wandering off it.*

**The rule: pick the task class honestly, then run its gates.** A *new system* that jumps
straight to implementation gets redesigned — coding is not step two. But a typo fix doesn't
need a Player-Value writeup. Match the gates to the work; when you drop a gate it's a named
choice, not a silent skip.

---

## The pipeline

```
Idea
  ↓
Player Value        — who is this for and what fantasy/feeling does it serve?
  ↓
Design              — write it down: behavior, rules, edge cases, success metric
  ↓
Technical Review    — what existing systems change? cross-cutting concerns? (see Decision Framework)
  ↓
Risk Review         — what could this break or bloat? what's the cheapest way to be wrong?
  ↓
Prototype           — rough, programmer-art, throwaway-if-needed. prove the loop.
  ↓
Playtest            — a real person, real hardware, real build. observe, don't ask leading questions.
  ↓
Revision            — change the DESIGN based on what the playtest showed
  ↓
Production Foundation — after HUMAN KEEP on a new game (skip if FOUNDATION APPROVED already)
  ↓
Production           — implement for real, against the approved ownership contract
  ↓
Code Review         — correctness, responsibility boundaries, dependency justification
  ↓
QA                  — verified in the artifact that actually ships (not just dev)
  ↓
Merge
  ↓
Postmortem          — on a surprise / kill / incident: what did this teach? (not every feature)
```

## Stage gates (definition of done per stage)

A feature does not advance until its current stage's gate is met:

| Stage | Gate |
|---|---|
| Player Value | Names the pillar it serves and the feeling it creates. If it serves none, stop. |
| Design | Written, with edge cases and a measurable success metric. |
| Technical Review | Impact on existing systems and cross-cutting concerns documented. |
| Risk Review | Biggest risk named; a way to test the assumption cheaply identified. |
| Prototype | Answers **one written gameplay hypothesis** in a greybox sitting (30–90 min for a verb; one or two evenings for a slice). No production art, no production architecture. **TECH PASS** possible. An LLM critic may not close this stage. |
| Playtest | Observed with a real player on a real build. Only a named human writes **HUMAN PASS** or **HUMAN FAIL**. |
| Revision | Design updated from playtest evidence (or explicitly confirmed unchanged). |
| Production Foundation | **FOUNDATION APPROVED** (Wyatt). Ownership contract, skeleton, change drill, named proof. Skip if the game already has it. |
| Production | Implemented against the approved contract; one responsibility per file. Blocked until FOUNDATION APPROVED on a new game. |
| Code Review | Passed review; every new dependency justified in writing. |
| QA | Verified in the deployed/shipped artifact, not just locally. |
| Postmortem | Only after a surprise, a killed prototype, or a production incident — capture the lesson in the *game* repo; promote to the OS if a later game re-derives it. |

## Say the feeling before you build

At Player Value / Design, describe the end result in human terms — the *feeling* and the
destination — before anyone writes code. "Add a menu" is an implementation task; "the title
screen should feel like a small arcade game, not a corporate app — big centered title,
one Start + one Settings, Enter-to-start, works on laptop and phone, don't build Settings
yet" is a destination. A destination gives the agent a target and gives you a way to reject
output that misses the feel without arguing about code. (Prompt in
[AI/Prompt-Library](../AI/Prompt-Library.md).)

## Evidence states (agents may not skip ahead)

An agent reports the **highest state the evidence supports**. It never awards itself HUMAN PASS.

| State | Who may write it | Evidence |
|---|---|---|
| **TECH PASS** | Agent | Headless or MCP run: no `SCRIPT ERROR`, no crash, a named scenario assert holds. |
| **AGENT REVIEW** | Agent (LLM critic) | Notes from **video or live frames**, against a written checklist. Not a close. |
| **HUMAN PLAYTEST OWED** | Agent, after TECH PASS | Named steps, local build, one issue. Seeded like Cart Clash playtest debt. |
| **HUMAN PASS** / **HUMAN FAIL** | Human only | They played the build. Their words in the game repo. |
| **KEEP** / **REWORK** / **KILL** | Human only | After HUMAN PASS or FAIL on the *question*, not on the slice looking complete. |
| **FOUNDATION TECH PASS** | Agent, after HUMAN KEEP | Ownership contract, production skeleton, change drill, and named headless/export proof are complete. |
| **FOUNDATION APPROVED** | Wyatt only | He reviewed the contract and evidence. Production feature work may begin. |

Slop Park’s “FINAL VERDICT: PASS” from screenshots and AI critics was a skip from AGENT REVIEW to KEEP. Frozen. Do not polish it.

LLM critics are useful when the model can **see** (GameDevBench: visual feedback raised pass rates). They score observables (“the weld spark fired,” “both pads move a body”). They do **not** score fun. A weak-vision critic is worse than none — it stamps theater.

## One gameplay hypothesis per prototype

If you have two gameplay hypotheses, build two prototypes. A prototype that tests movement + experimental physics + welding + terrain + HUD + comedy tests nothing (Slop Park). The one-hypothesis rule controls prototype scope, not chat cadence. Batch related design questions. Ask one question alone only when its answer selects a materially different direction or makes further work unsafe.

Fill this before any prototype code:

```text
QUESTION:     The one thing this must prove (binary if possible).
CORE VERB:    The single action the player repeats.
PLAYERS:      1 / 2 local. If 1.0 is co-op, the prove unit is 2.
THROWAWAY?:   yes → hard-code, delete after. no → minimal names you can grow.
TIMEBOX:      30–90 min (verb) or one sitting (slice). Stop when it rings.
KEEP IF:      a fresh player does the verb unprompted and repeats without being asked.
KILL IF:      it only works after you explain it, or fun needs systems far beyond this sitting.
```

## Approved iteration grant

The approved plan is the boundary for normal reversible work. The plan must name the goal,
files, owners and public seams, risk boundary, and verification plan. After human approval, the
agent may implement, tune, debug, run repeated edit → run → observe cycles, repair tests caused
by the approved change, and add documentation that the result makes necessary. These actions do
not need a new approval while they stay inside the named boundary.

Ask for a new approval when the work crosses the goal, file, owner, risk, or verification
boundary. A grant never authorizes a new dependency, architecture or ownership change, purchase,
destructive or external action, human verdict, commit, merge, or push.

## Playable-build cadence

Every concept proof or gameplay slice names the next player-visible build. Keep the slice small
enough to reach that build in one development sitting when practical. Test several tuning values
inside the approved slice instead of opening a new planning cycle for each value. If plans,
records, frameworks, or harnesses continue to grow without another playable build, cut scope.

The minimum concept-proof record is: one hypothesis, player-visible behavior, scope, KEEP or
rework condition, technical floor, and current evidence state. Write detailed records after
evidence exists. Use the full Decision Framework only for a durable product, architecture,
process, dependency, authority, or ownership choice.

Spike in `prototypes/<idea>/` or a throwaway branch. KEEP opens the **Production Foundation
Gate** below; it does not authorize copying the spike into production. (Shape adapted from
`prototype-fast` in awesome-gamedev-agent-skills, Godot **4.7** baseline.)

## KEEP opens the Production Foundation Gate

A concept that earns **HUMAN KEEP** does not advance directly to production features. Its next
top task is `production-foundation`: a deliberate re-foundation while the game is still small.
This gate runs once when a prototype becomes a product. Run it again only when a foundational
lock changes, such as engine, target platform, multiplayer authority, or save model.

The gate has five required parts:

1. **Dispose deliberately.** Record the KEEP evidence and whether the prototype is deleted,
   archived, or retained as reference. Prototype code is not production source by default.
2. **Name ownership.** In the game’s `PROJECT.md`, name each initial system, what it owns, its
   public seam, allowed dependencies, and forbidden reaches.
3. **Set cross-cutting boundaries.** Decide input, time, save, multiplayer authority,
   diagnostics, and export where they apply. Mark a concern N/A with a reason instead of
   designing a system the game does not need.
4. **Build the production skeleton.** Prove launch → input → the kept verb → reset, plus
   diagnostics, a headless run, and a Windows export. Do not add content breadth.
5. **Run a change drill.** Add the smallest representative second case through the declared
   seams, then review the diff and architecture contract. A data variant or alternate input
   path is enough; this is not permission to build another feature.

An agent may report **FOUNDATION TECH PASS** with named evidence. Only Wyatt writes
**FOUNDATION APPROVED**. Production feature work is blocked until that approval.

The gate fails when any of these shapes appears:

- External code reaches into another scene’s child hierarchy.
- Mutable state has more than one owner.
- Correct behavior depends on string remapping or undocumented initialization order.
- A simple change requires edits inside three unrelated owners.
- A file or system needs “and” to describe its responsibility.

Fix the boundary and rerun the affected proof. Do not waive a failed shape into deferred debt
while the foundation is still small.

## Killing a prototype is a valid outcome

A prototype's job is to answer a question: *is this fun / is it worth building?* If the
answer is no, **killing it is success, not failure** — it did its job cheaply. The expensive
mistake is sinking production work into a loop that never earned it. Kill it when: it isn't
fun rough and you can't see why it would be finished; it fights a design pillar; or the cost
to make it good keeps climbing every playtest. Log the kill (Decision Framework) so the
"why" survives.

## The merge gate: two independent gates

Nothing merges until it passes **both** gates. "The agent said it's good" is not a reason to
merge.

- **Gate 1 — does it actually work (subjective).** The end-to-end flow works with no
  narration or "you have to do X first," zero console errors, real error handling (not just
  the happy path), and it looks/feels right. Watch for red-flag phrases: *"it works if you
  just…"*, *"it's basically done"*, *"it's only a prototype"* three weeks in.
- **Gate 2 — is it sound (deterministic).** The human **reads the diff** (non-negotiable),
  tests pass, no unapproved dependencies snuck in, and no secrets/keys are in the change.

**Verify in the shipped artifact — the proof ladder** (strongest to weakest):

1. A real person using the live product.
2. The deployed build on the target device, confirmed by a **production marker** — add a
   unique string to the change, deploy, fetch the live URL *with no-cache headers*, and grep
   the response for the marker before you test the feature.
3. A local build you actually ran.
4. "The AI says it works." / AGENT REVIEW / a gauntlet vs other games' stills. ← not proof.

Local files don't count. Build folders don't count. A browser tab from ten minutes ago
doesn't count. (This ladder is Lesson L-07 made concrete.)

## Task classes — match the gates to the work

The full pipeline is the path for a **new system**. Most work is smaller. Pick the honest
class and run its gates; anything you drop is a named choice, not a silent skip.

| Class | Gates it runs | Consciously drops |
|---|---|---|
| **new-system** | the full pipeline | — |
| **feature** | Player Value → Design → Prototype → Playtest → Production → Code Review → Validate | Technical/Risk writeups when the system is well understood |
| **bug** | Root-cause (L-14) → Fix → Code Review → Validate | design · prototype · playtest |
| **tune / feel** | Prototype (the tune panel) → Playtest → apply → Validate | design docs · heavy review |
| **chore / docs** | Code Review → Validate | everything upstream |
| **concept-prove** | One-hypothesis prototype → TECH PASS → (optional AGENT REVIEW) → human play → KEEP/REWORK/KILL | production architecture · skill packs · net essays |
| **production-foundation** | HUMAN KEEP → ownership contract → skeleton → change drill → FOUNDATION TECH PASS → human approval | new content · production art · speculative systems |

Two gates **never** drop, whatever the class: **root-cause before the fix** (L-14) and
**verify in the shipped artifact** (the proof ladder). If you're unsure which class it is,
size up — a mislabeled `chore` that was really a `new-system` is how Cart Clash bled time.
