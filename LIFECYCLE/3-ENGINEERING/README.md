# 3 · Engineering

**The standards layer** — how code, scenes, folders, naming, and git are done here, so every
agent builds the same way.

- The load-bearing rules already live in `FOUNDATION/` (one responsibility per file, module
  boundaries, verify-the-shipped-build) and `AI/` (model routing, the working loop).
- This folder holds engine- and project-agnostic standards as they're written: folder layout,
  naming, scene composition, git conventions.

Grows with real standards. Until a standard is written, the binding rules are the Manifesto +
`FOUNDATION/Anti-Patterns.md`.
