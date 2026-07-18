# Anti-Patterns

*A diff-review checklist of named failure modes. Run these Detection lines against a change
before a human reads it — the agent that wrote the code has already convinced itself it's
fine; this list hasn't. The narrative "why," recovery, and prevention for each smell live in
its cross-referenced Lesson or Manifesto rule — this file is the **detector**, not the essay.*

> Seeded from the Cart Clash build; Detection lines are written to be checkable (grep-able,
> countable, or a one-sentence test). Add rows as new shapes recur.

---

## The checklist

| # | Smell | Detection — the checkable tell | Why (ref) |
|---|---|---|---|
| 1 | **God Files** | Describing the file needs the word "and"; or it's the top git-churn hotspot, >~800 lines, or imports from >3 unrelated domains. | L-04 |
| 2 | **Hidden State** | The last fix here was a comment, not a guardrail; or state is set in module A and read in B with nothing linking them; or "works in dev, stale / silent no-op in prod." | L-08, L-02 |
| 3 | **Temporal Coupling** | You can name a "must run before" that nothing enforces; a defer/flush/order comment is the only thing holding it up; swapping two adjacent lines changes behavior. | L-02, L-12 |
| 4 | **Magic Numbers** | A bare literal in a branch with no named constant; grep the literal and it's in >1 file; you edit source (not data) to change feel. | L-17 |
| 5 | **Feature Creep** | You can't name the pillar a feature serves; it's not in the design doc and no decision-log entry explains its arrival. | Manifesto §6 |
| 6 | **One-Off Exceptions** | A name-checked conditional (`=== 'zanzibar'`) inside otherwise-generic logic; roughly one special-case per variant. | L-13, L-17 |
| 7 | **Premature Optimization** | No profile or playtest pointed at it before it was optimized; the fast path is the default with no fallback; no measured before/after. | L-09 |
| 8 | **Refactoring Without Purpose** | The diff's file list isn't a subset of the plan's; "while I was here"; a structural refactor landing *before* the gate it could break. | L-11 |
| 9 | **Dependency Collecting** | A dep used in exactly one place / for one function; it entered with no decision-log line; you can name a <20-line hand-roll that replaces it. | Manifesto §5 |
| 10 | **Menu Explosion** | The menu file is a top-3 churn hotspot; options-added outpaces options-removed; a setting no playtester has ever changed. | Manifesto §6, L-06 |
| 11 | **Singleton Abuse** | Runtime (non-init) code assigns to a global / `CONFIG`; two stores expose the same state; a test fails unless you reset a global first. | Manifesto §4 |
| 12 | **Architecture Astronautics** | An interface / factory / plugin system with one implementation and no second on the roadmap; a folder of stub files; you can't name the concrete case it serves *today*. | Manifesto §5 |
| 13 | **Manager-Manager Syndrome** | Two+ `*Manager` / `*System` / `*Controller` in one call path, at least one of which only delegates; a class with no behavior of its own. | Manifesto §4 |

## How to use it

- **During diff-review:** run each Detection line against the change. A match is not
  automatically a bug — decide load-bearing vs. debt, then fix it or justify it in writing.
- **For the "why," recovery, and prevention:** follow the cross-ref. The Lessons carry the
  full story; this table just catches the shape.
- **Adding one:** when a shape recurs across projects, add a row with a *checkable* Detection
  line and a cross-ref. A catalog you can grep beats one you have to read.
