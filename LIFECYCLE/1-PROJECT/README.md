# 1 · Project

**Define the game before building it.** Write the contract so no agent or session re-decides
the stack, broadens scope, or "fixes" unrelated things.

- Start from `GAME_TEMPLATE/` — copy `AGENTS.md`, `PROJECT.md`, `Tasks.md`, and the thin
  pointers (`GROK.md`, `CLAUDE.md`, `GEMINI.md`, `.cursorrules`) into the new repo, and add
  RedShiftOS as a git submodule at `./RedShiftOS`, pinned to a named commit (Decision #9).
  Sibling checkout is not supported.
- Fill `PROJECT.md`: concept, target player, stack, first shippable version, primary tools,
  cut list.
- **Gate:** you can name the core loop, what "done" means for milestone 1, and what's
  explicitly cut.

Related: Manifesto §1 (prove it first), `FOUNDATION/Decision-Framework.md` for the first big
calls.
