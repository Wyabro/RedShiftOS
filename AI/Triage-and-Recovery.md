# Triage & Recovery

*When an agent is stuck in a hallucination / fix-fail loop, escalate deliberately — don't
just say "try again." Blind retries dig the hole deeper.*

---

## The escalation ladder

Climb it in order; don't skip to the bottom, don't loop forever near the top:

1. **Agent self-correction (structured).** Make it restate the goal, list exactly what it
   already tried, and what it expected vs. observed. Often surfaces the wrong assumption.
2. **Instrument for ground truth.** Add logging/telemetry and get real observed data instead
   of the agent's guess about what's happening. Treat observed output as truth — if the bug
   is still visible, it's still real (don't let the agent explain it away).
3. **External diagnostic.** Open a *fresh, high-context* window and frame it as an outside
   auditor reviewing only the diff + plan — no attachment to the code that was written. The
   agent that wrote the code has already convinced itself it's correct.
4. **Hard revert.** Reset to the last good checkpoint and re-decompose the problem into
   smaller, verified steps.

## The stopping rule

After ~3 structured attempts, **stop prompting.** The strategy has failed — more attempts
won't rescue it. Revert to a checkpoint, break the work smaller, and restart with stricter
gates. Rescuing a deeply corrupted session is almost always more expensive than starting
clean.

## What makes recovery cheap

- **Checkpoint commits** before risky runs, and agent work on a feature branch — so a revert
  costs nothing (see Lesson L-11).
- **A handoff** written the moment context degrades (see [Session Handoffs](../LIFECYCLE/4-IMPLEMENTATION/Session-Handoffs.md)).
- **Small slices**, so "the last good state" is never far behind.
