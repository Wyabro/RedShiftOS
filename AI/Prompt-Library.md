# Prompt Library

*Reusable prompts for the moves that keep agents on the rails. Copy, fill the `<brackets>`,
paste. Keep them short — a prompt you have to re-read is too long.*

---

## Batch blocking questions before planning

> Before you write the plan, batch the blocking questions you need answered: stack, constraints,
> edge cases, and what “done” means. Make reversible assumptions explicit. Ask one question alone
> only if its answer selects a materially different direction or makes work unsafe. Do not turn
> one gameplay hypothesis into one chat message at a time.

## Plan before code (the checkpoint)

> Explore the relevant code and report back **without editing anything**: goal, exact files,
> ownership and risk boundaries, approach, and verification. After approval, use the approved
> iteration grant for normal reversible implementation, tuning, focused debugging, and repair
> inside that boundary. Ask again only when work crosses it. Do not commit or push without a
> separate instruction.

## Describe the end result / feel

> I want `<feature>` to feel like `<reference / mood>`, not `<anti-reference>`.
> End result:
> - `<concrete, observable bullet>`
> - `<concrete, observable bullet>`
> Do **not** build `<out-of-scope thing>` yet. Give me a destination, not just code.

## Writer–reviewer (fresh context)

> You are an outside reviewer. Look **only** at this diff and the plan it claims to implement.
> Flag scope creep, unrelated changes, broken assumptions, and anything the plan didn't call
> for. You did not write this code — don't defend it.

## Diagnose before fixing

> The bug is still visible, so it's still real — don't explain it away. Give me three possible
> root causes, the smallest test for each, and what I'd observe if each were true. We agree on
> the diagnosis **before** you propose a fix.

## Agent review (not a human gate)

> You are an AGENT REVIEW, not a playtest. Watch the video / live frames against this
> QUESTION: `<question>`. Score only observables: is the player on screen, did the core verb
> change the named state, did a second body move if this is 2p, any SCRIPT ERROR. Do **not**
> score funny, hooked, or comparisons to other games. End with HUMAN PLAYTEST OWED. Never
> write HUMAN PASS, KEEP, or KILL.

## One-hypothesis prototype brief

> Fill this one gameplay hypothesis: QUESTION / CORE VERB / PLAYERS (1 or 2 local) / THROWAWAY?
> / TIMEBOX / KEEP IF / KILL IF. Batch related questions before the plan. Greybox only. Stock
> Godot 4.7 Jolt. No GDExtension. Spike in prototypes/ or a throwaway branch. Do not stamp
> HUMAN PASS.

## Verify in the shipped artifact

> Add a unique marker to this change, deploy, then fetch the live URL with no-cache headers and
> grep the response for the marker. Only after you've confirmed the marker is live do you test
> the feature. Local files and old browser tabs don't count.

## Recover a stuck session

> Stop. Restate the goal, list everything you've tried and the result of each, and what you
> expected vs. what actually happened. Do not attempt another fix until we've done this.
