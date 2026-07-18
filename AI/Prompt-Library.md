# Prompt Library

*Reusable prompts for the moves that keep agents on the rails. Copy, fill the `<brackets>`,
paste. Keep them short — a prompt you have to re-read is too long.*

---

## Interview before planning

> Before you write any plan or code, ask me the questions you need answered to do this well —
> stack, constraints, edge cases, what "done" means. Don't assume; a capable agent asks first.

## Plan before code (the checkpoint)

> Explore the relevant code and report back **without editing anything**: what you'd change,
> which files, the approach, the risks, and how you'll verify it. Wait for my approval before
> implementing.

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

## Verify in the shipped artifact

> Add a unique marker to this change, deploy, then fetch the live URL with no-cache headers and
> grep the response for the marker. Only after you've confirmed the marker is live do you test
> the feature. Local files and old browser tabs don't count.

## Recover a stuck session

> Stop. Restate the goal, list everything you've tried and the result of each, and what you
> expected vs. what actually happened. Do not attempt another fix until we've done this.
