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
Production           — implement for real, against the OS's architecture rules
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
| Prototype | Playable rough version exists — no production art, no production code. |
| Playtest | Observed with a real player on a real build. |
| Revision | Design updated from playtest evidence (or explicitly confirmed unchanged). |
| Production | Implemented against architecture rules; one responsibility per file. |
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

Two gates **never** drop, whatever the class: **root-cause before the fix** (L-14) and
**verify in the shipped artifact** (the proof ladder). If you're unsure which class it is,
size up — a mislabeled `chore` that was really a `new-system` is how Cart Clash bled time.
