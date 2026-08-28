# PROJECT.md — <GAME NAME>

**This game's contract.** Fill it in before asking an agent to build anything serious — it's
how you stop every new session from re-deciding the stack, broadening scope, or "fixing"
unrelated files. Keep it short, dense, and current; if a rule needs constant rewriting, it
belongs in a separate doc, not here.

---

## Project

- **Name & one-sentence concept:**
- **Visible hook** (what a 5-second GIF shows):
- **Comparable games** (2–3, including what we are *not*):
- **Target player:**
- **Target platform & hardware:**
- **Price band (if Steam):**
- **Current milestone — what success looks like:**
- **Current evidence state:** *(TECH PASS / AGENT REVIEW / HUMAN PLAYTEST OWED / HUMAN PASS / HUMAN FAIL / KEEP / REWORK / KILL)* — human writes PASS/FAIL/KEEP/KILL

## Stack

- **Engine / framework + language:** Godot **4.7.x** + GDScript *(or name the exception)*
- **Physics:** stock Jolt *(no GDExtension in prove)*
- **Player count at 1.0:** 1 / 2 / 4
- **Authority model** (if 2+): host-authoritative *(or written alternative)*
- **Backend / database / hosting:**
- **Pinned Godot MCP** (name + release tag, or “headless floor only”):
- **AI coding tools in use:**
- **Runtime AI / model use (if any):** none for M1 unless written here

## First shippable version (the prove)

- **The one question:**
- **Core verb:**
- **Required scenes / systems** (greybox only):
- **What counts as KEEP:** a fresh player does the verb unprompted and repeats without being asked
- **Kill if:**

If 1.0 is co-op, this prove is **two local players** on one machine (L-22). Steam, lobbies,
and migration are not this section.

## Production foundation — fill only after HUMAN KEEP

Before KEEP, this section is **NOT OPEN**. After KEEP, the next task is the Production
Foundation Gate in `RedShiftOS/FOUNDATION/Feature-Lifecycle.md`, not a production feature.

- **Foundation state:** NOT OPEN / IN PROGRESS / FOUNDATION TECH PASS / FOUNDATION APPROVED
  *(only Wyatt writes APPROVED)*
- **KEEP evidence** (person, build, date, observed behavior):
- **Prototype disposition** (delete / archive / reference only):
- **Composition root** (the one place that wires top-level owners):

### Ownership contract

| Owner | Owns | Public seam | May depend on | Must not reach into |
|---|---|---|---|---|
| | | | | |

For Godot: a scene owns its descendants; outside code talks to its root API. Signals travel
up, calls travel down, and a parent wires siblings. Each mutable state value has one owner.
Autoloads are platform services, not a general gameplay-state bucket.

### Cross-cutting boundaries

| Concern | Owner and decision, or N/A with reason |
|---|---|
| Input | |
| Time / pause | |
| Save | |
| Multiplayer authority | |
| Diagnostics / export | |

### Foundation proof

- **Production skeleton route:** launch → input → kept verb → reset
- **Change drill:**
- **Verification commands and expected signals:**
- **Windows export tested by:**

If this completed contract later outgrows `PROJECT.md`, move it to `ARCHITECTURE.md` and link
it here. Do not create that file before the gate opens.

## Cut list

- **Deferred until KEEP:**
- **Cut if blocked:**
- **Post-launch:**

## AI rules & patterns that already burned us

The RedShiftOS Manifesto is binding, and Lessons Learned are binding when the task pulls them
(see `AGENTS.md` load order). This section is for *this game's* extras — engine-specific rules,
forbidden patterns, and the specific mistakes this project has already paid for:

- Agents do not write HUMAN PASS / KEEP / KILL.
- Godot skills, if installed, are the **4.7** allow-list in `RedShiftOS/LIFECYCLE/3-ENGINEERING/Godot.md` — not a 60+ pack.
- Third-party (Asset Store first, then Asset Library, Kenney, Poly Haven, GitHub): license row in the provenance table **same commit** as the files. Prefer 4.7; 4.x only with a 4.7 smoke + tweak note. No GPL/NC toward Steam without a written accept.
-
