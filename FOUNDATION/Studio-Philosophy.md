# Studio Philosophy

*The Manifesto says **what** to do. This says **why** — the beliefs underneath the rules, and
the lenses for the decisions the rules don't cover. When a rule and reality disagree, come
back here. If a rule ever stops serving these beliefs, change the rule.*

---

## Why we build this way

**Why we prototype.** You discover the game by playing it, not by planning it. A cheap, rough
version answers "is this fun / is this worth it" before the answer gets expensive. Every hour
spent making a bad idea *nice* is an hour lost. Prove it rough, or kill it rough.

**Why AI is a collaborator, not the author.** Agents are fast, tireless, and context-blind.
They optimize whatever's in front of them — brilliantly, locally, and often against the long
game. The human holds the vision, the pillars, and the taste; the OS is how the studio hands
its context to the agents so their local moves add up to a globally good game. That gap —
locally optimal, globally expensive — is the entire reason RedShiftOS exists.

**Why we optimize for iteration.** A game is found through hundreds of small changes. So the
most valuable property of any system, file, or decision is *how cheaply it can change*.
Architecture that's elegant but rigid loses to architecture that's plain but fast to edit.
Structure serves the next change, not the diagram.

**Why simplicity.** Every line, dependency, and abstraction is something you'll carry for
years and something an agent can misuse. The simplest thing that holds is not a compromise —
it's the target. Cleverness is a loan against future-you, at interest.

**Why we write things down.** Memory is lossy and agents start from zero every session.
Documentation isn't overhead *on* the work — it *is* part of the work, because it's what turns
one person's hard-won context into something the whole studio (human and AI) can build on. The
OS gets smarter only because we write down what shipping taught us.

## Decision lenses

Not rules — lenses. Hold a hard decision up to these:

- **Information belongs near where it's used.** Config, state, and logic drift toward the code
  that reads them. Distance between a value and its use is future confusion.
- **Optimize for deleting code.** The best change is often a removal. Prefer designs that make
  a feature easy to *cut*, not just easy to add. Deletable is a feature.
- **Architecture should enable iteration.** If a structure makes the next change harder, it's
  wrong — however clean it looks.
- **Prefer boring solutions.** Novelty is a cost you pay forever. Reach for the well-worn tool
  unless the problem genuinely demands the exotic one — and justify the exotic one in writing.
- **Players are always telling the truth — their explanations aren't.** What a playtester
  *does* and *feels* is data. What they say *caused* it, or how they'd *fix* it, is a guess.
  Trust the behavior; diagnose the cause yourself (Lesson L-14).

---

*These beliefs are the source the rules compile from. Read them when a rule feels wrong — one
of the two needs to change.*
