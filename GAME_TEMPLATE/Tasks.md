# Tasks — <GAME NAME>

**This file is the source of truth for active work.** The agent may only work on the single top
task under "In progress." To change focus, the human edits this file first and moves a task to
the top. This keeps priority and evidence state a human decision, not something the agent picks.

Each in-progress line names its **evidence state** (Feature-Lifecycle). Agents update TECH PASS
/ AGENT REVIEW / HUMAN PLAYTEST OWED / FOUNDATION TECH PASS. Only the human writes HUMAN PASS,
HUMAN FAIL, KEEP, REWORK, KILL, or FOUNDATION APPROVED.

The default for parallel work is a separate worktree. Same-checkout work is allowed only when
Wyatt explicitly approves it and this file names an exact non-overlapping file allowlist, no
shared mutable owner, no Git state-changing commands, a stop-on-overlap rule, and a focused
integration review. A support lane may not add a second gameplay hypothesis or change the main
gameplay priority.

---

## In progress

- <the one task — include QUESTION + state, e.g. "Prove weld-on-crate (QUESTION: is grab→stick funny?). State: HUMAN PLAYTEST OWED">

## Up next

-

## Approved parallel support

- <optional support card — include owner, separate-worktree or explicitly approved same-checkout mode, exact file allowlist, forbidden files/owners, stop-on-overlap rule, and integration reviewer>

## Parked / blocked

-

## Recently done

-
