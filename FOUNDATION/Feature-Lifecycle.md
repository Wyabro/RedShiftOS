# Feature Lifecycle

*The path every feature follows, from idea to merged. This is the development heartbeat.
Each stage has one narrow objective, which is exactly what makes AI agents consistent — a
step with a single goal is a step an agent can't wander off from.*

**The rule: no skipping, no exceptions.** A feature that jumps straight to implementation is
a feature that will be redesigned. The whole point of Cart Clash's expensive lessons was
that coding is not step two.

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
Production           — implement for real, against the OS's architecture rules
  ↓
Code Review         — correctness, responsibility boundaries, dependency justification
  ↓
QA                  — verified in the artifact that actually ships (not just dev)
  ↓
Merge
  ↓
Postmortem          — what did this teach? new Lessons-Learned entries if any.
```

## Stage gates (definition of done per stage)

A feature does not advance until its current stage's gate is met:

| Stage | Gate |
|---|---|
| Player Value | Names the pillar it serves and the feeling it creates. If it serves none, stop. |
| Design | Written, with edge cases and a measurable success metric. |
| Technical Review | Impact on existing systems and cross-cutting concerns documented. |
| Risk Review | Biggest risk named; a way to test the assumption cheaply identified. |
| Prototype | Playable rough version exists — no production art, no production code. |
| Playtest | Observed with a real player on a real build. |
| Revision | Design updated from playtest evidence (or explicitly confirmed unchanged). |
| Production | Implemented against architecture rules; one responsibility per file. |
| Code Review | Passed review; every new dependency justified in writing. |
| QA | Verified in the deployed/shipped artifact, not just locally. |
| Postmortem | Lessons captured; OS docs updated to match reality. |

## Say the feeling before you build

At Player Value / Design, describe the end result in human terms — the *feeling* and the
destination — before anyone writes code. "Add a menu" is an implementation task; "the title
screen should feel like a small arcade game, not a corporate app — big centered title,
one Start + one Settings, Enter-to-start, works on laptop and phone, don't build Settings
yet" is a destination. A destination gives the agent a target and gives you a way to reject
output that misses the feel without arguing about code. (Prompt in
[AI/Prompt-Library](../AI/Prompt-Library.md).)

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
4. "The AI says it works." ← not proof.

Local files don't count. Build folders don't count. A browser tab from ten minutes ago
doesn't count. (This ladder is Lesson L-07 made concrete.)

## Scaling the pipeline

The pipeline is fixed; its *weight* scales with the feature. A one-line tuning tweak runs
through the same stages in minutes (player value: obvious; prototype: the tune panel;
playtest: feel it). A new game mode takes days. The stages don't get skipped — they get
sized. When a stage genuinely doesn't apply, that's a decision to *log*, not to skip
silently.
