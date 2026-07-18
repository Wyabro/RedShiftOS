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

## Scaling the pipeline

The pipeline is fixed; its *weight* scales with the feature. A one-line tuning tweak runs
through the same stages in minutes (player value: obvious; prototype: the tune panel;
playtest: feel it). A new game mode takes days. The stages don't get skipped — they get
sized. When a stage genuinely doesn't apply, that's a decision to *log*, not to skip
silently.
