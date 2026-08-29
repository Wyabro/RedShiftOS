# GAME_TEMPLATE

The starting point for a new Red Shift Studios game wired to RedShiftOS. Every new game starts here.

## Starting a new game

1. **Create the game repo.**
2. **Add RedShiftOS as a git submodule at `./RedShiftOS` and pin a named commit.**
   `git submodule add <RedShiftOS-url> RedShiftOS`
   Then checkout the pin inside `RedShiftOS/`. Sibling checkout is not supported
   (Decision #9): every pointer and opener uses `RedShiftOS/...`.
3. **Copy `AGENTS.md`, `PROJECT.md`, `Tasks.md`, and the pointer files** (`GROK.md`,
   `CLAUDE.md`, `GEMINI.md`, `.cursorrules`) from here into the game repo root. If a tool
   auto-reads a different filename, add another thin pointer. Do not restate `AGENTS.md` in it.
4. **Fill `PROJECT.md`** — hook, comps, player count, authority, Godot **4.7.x**, the one
   question, and which tools are primary on this game. Do this before asking an agent to
   build anything serious.
5. **Prove, then structure.** First sitting = greybox core verb (`prototypes/` or a throwaway
   branch). Stock Godot 4.7 Jolt. Two local players if 1.0 is co-op. Human plays. KEEP / KILL.
   After KEEP, run the Production Foundation Gate in `RedShiftOS/FOUNDATION/Feature-Lifecycle.md`.
   During the gate, pin only the MCP and task-matched 4.7 skills it needs
   (`RedShiftOS/LIFECYCLE/3-ENGINEERING/Godot.md`). Only after FOUNDATION APPROVED: production
   feature cards and broader tests.
6. Run the prove through `RedShiftOS/FOUNDATION/Feature-Lifecycle.md` evidence states.
   Agents may reach TECH PASS / AGENT REVIEW. They do not write HUMAN PASS.

From then on, every agent session reads `AGENTS.md` first, which loads the OS — so every
agent starts knowing how Red Shift Studios builds games, not just today's prompt.

## Create records at their trigger

Do not seed empty files at copy-in (Decision #1). Create each record when it first applies:

| Record | Create when |
|---|---|
| `decisions.md` | First significant game decision |
| Provenance tables | First external or AI-assisted asset — `RedShiftOS/LIFECYCLE/6-PRODUCTION/Assets-and-Provenance.md` |
| Latest handoff (`HANDOFF.md`, `STATUS.md`, or a `Tasks.md` note) | End of the first session — `RedShiftOS/LIFECYCLE/4-IMPLEMENTATION/Session-Handoffs.md` |
| Export / release notes | After FOUNDATION APPROVED, on the first real export |

Change the RedShiftOS pin only through a named game decision plus a cold-start smoke test
(an agent finds the current task and evidence state with no oral correction).
