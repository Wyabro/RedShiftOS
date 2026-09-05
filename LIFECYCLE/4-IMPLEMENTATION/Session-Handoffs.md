# Session Handoffs

*End every working session with a handoff. It looks too simple until you need it — it kills
the 10–30 minute re-onboarding tax and stops the next session from undoing the last one's
good work.*

---

## The format — three lines

```
CHANGED:  what was modified, in plain language.
BROKEN:   known issues / anything intentionally left half-done.
NEXT:     the single next action to take.
```

That's it. More than three lines is fine; fewer means you skipped one.

## Where it goes

The handoff is recovery context, not a second task tracker. `Tasks.md` is authoritative for the
active task, evidence state, and approved support lane. `PROJECT.md` holds durable product and
architecture facts. `decisions.md` holds durable game decisions. The handoff records only the
latest change, known breakage, and next action needed to resume.

- Commit it (a `HANDOFF.md`, or append to `Tasks.md` / a `sessions/` log), so the next
  session — human or agent — reads it as part of the load order.
- In a game repo, this is what `STATUS.md` / the latest handoff refers to in the AGENTS.md
  load order. (Cart Clash's own `docs/STATUS.md` already works exactly like this — codify it.)

## When to write one early

If context feels degraded mid-session — the agent is looping, contradicting itself, or
forgetting decisions — **stop and write the handoff now**, then restart from a fresh context.
Rescuing a degraded session costs more than restarting from a clean one (see
[Triage & Recovery](../../AI/Triage-and-Recovery.md)).
