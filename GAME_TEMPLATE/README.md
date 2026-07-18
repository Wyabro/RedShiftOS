# GAME_TEMPLATE

The starting point for a new RedShift game wired to RedShiftOS. Every new game starts here.

## Starting a new game

1. **Create the game repo.**
2. **Make RedShiftOS available beside it** — either a git submodule at `./RedShiftOS`
   (`git submodule add <RedShiftOS-url> RedShiftOS`) or a sibling checkout next to the repo.
3. **Copy `AGENTS.md` and `PROJECT.md`** from here into the game repo root, and add a thin
   `CLAUDE.md` that points at `AGENTS.md` (Claude Code reads `CLAUDE.md`; other agents read
   `AGENTS.md` / their own rules path — point them all here).
4. **Fill in `PROJECT.md`** — the contract. Do this before asking an agent to build anything
   serious.
5. **First feature = the core loop**, run through `RedShiftOS/PROCESS/Feature-Lifecycle.md`.
   Prove it's fun rough before you build anything for real (Manifesto §1).

From then on, every agent session reads `AGENTS.md` first, which loads the OS — so every
agent starts knowing how RedShift builds games, not just today's prompt.
