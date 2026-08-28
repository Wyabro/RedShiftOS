# GAME_TEMPLATE

The starting point for a new Red Shift Studios game wired to RedShiftOS. Every new game starts here.

## Starting a new game

1. **Create the game repo.**
2. **Make RedShiftOS available beside it** — either a git submodule at `./RedShiftOS`
   (`git submodule add <RedShiftOS-url> RedShiftOS`) or a sibling checkout next to the repo.
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
