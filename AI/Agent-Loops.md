# Agent Loops

*How an agent iterates on a game without awarding itself a human gate. Pull this when
prototyping, wiring Godot, or when a session is about to stamp “done.”*

RedShiftOS v0.1 had principles and no operational loop. Agents filled the gap with a
screenshot gauntlet and called it PASS (L-18). This page is the loop we trust, ranked by
**evidence**, not by README star count.

---

## The loop (every engine)

```
one question → edit → run → observe → assert (TECH PASS) → AGENT REVIEW (optional)
    → HUMAN PLAYTEST OWED → HUMAN PASS / FAIL → KEEP / REWORK / KILL
```

Stop at ~45 minutes or 3 failed approaches (`AI/Triage-and-Recovery.md`). Write a handoff.
Do not start a critic gauntlet to “unblock” a timebox.

| Step | Proof it belongs | What it is not |
|---|---|---|
| Headless / compile / `SCRIPT ERROR` | Cart Clash qa; Slop Park probes that *did* catch physics bugs | Fun |
| Run + observe (log, screenshot, JSON state, **video**) | GameDevBench: visual feedback raised pass rates ~10 pts. Best agents still fail ~⅓–½ of Godot tasks. | HUMAN PASS |
| Deterministic input + clock-step | `godot-mcp-go` / `satelliteoflove/godot-mcp` — mechanic checks | Enjoyment |
| LLM critic with vision | Useful as **AGENT REVIEW** if the model can see (L-18) | KEEP / KILL |
| Named human on a local build | Cart Clash: every behavior change seeds playtest debt; Wyatt can revert | Optional |

Nobody has public proof that an agent shipped a Godot Steam co-op. Do not write the OS as if
they have. Cart Clash is this studio’s proof that **TECH PASS ≠ HUMAN PASS**, and that 2-client
smoke is a real gate, not an essay.

## LLM critic protocol (AGENT REVIEW)

Use a critic when TECH PASS is green and you want a cheap second look **before** bothering
Wyatt. Prefer a model with documented vision, not the one that failed Slop Park stills.

**May score (observables):**

- Is the player body on screen?
- Did the core verb change a node/state the checklist named?
- Did a second local player/body exist and move (if PROJECT.md is 2p)?
- Obvious SCRIPT ERROR / red error overlay?

**Must not score:**

- Funny / hooked / “Horde BEATEN” / “PEAK TIED”
- KEEP, KILL, HUMAN PASS
- Comparisons to other games from stills

**Input:** short **video** or live frames from a real play, plus the written QUESTION. Not a
1 Hz PNG dump timed for the effect (L-21).

**Output:** a notes file in the game repo (`playtest/agent-review-<id>.md`) that ends with
`HUMAN PLAYTEST OWED` — never `PASS`.

## Floor (always)

Works on Windows. No MCP required.

1. Import: `godot --headless --path . --import`
2. Run a named scene or `--script` smoke.
3. Fail the wave on `SCRIPT ERROR`, parse errors, or crash.
4. If the game is 2p (L-22), a second instance or second input map is part of the floor as
   soon as two bodies exist — not after content.

Godot-specific commands and the MCP pin live in
[`LIFECYCLE/3-ENGINEERING/Godot.md`](../LIFECYCLE/3-ENGINEERING/Godot.md). Engine is
**Godot 4.7.x** only.

## What we will not run

- Whole-game critic loops vs PEAK / Horde / RV There Yet from screenshots
- Installing 60+ skills into the OS or a prove repo (Anti-Pattern 17)
- Engine forks (Solers and similar) as a substitute for the loop
- BRIEFING generators / architecture.json until a game past KEEP actually needs them
  (ROADMAP: still no automation wizard)
