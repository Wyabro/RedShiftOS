# Development Manifesto

*How RedShift Studios builds games. This is operational, not motivational — every line is a
rule you can be held to, not a poster. If it grows past two pages it has become a rulebook;
cut it back.*

> **Draft — needs Wyatt's voice.** These principles are seeded from the Cart Clash
> experience. Rewrite them in your words, cut the ones you don't believe, and add the ones
> you do. A manifesto you didn't write won't get followed.

---

1. **Prove the loop before you build the game.** Fun is validated with programmer art and a
   rough prototype — never assumed, never polished into existence. If the core loop isn't
   fun rough, it won't be fun finished.

2. **Prototype before architecture.** Requirements → prototype → validate → *then* production
   architecture. Committing to expensive structure before the idea is proven is how Cart
   Clash's code and design co-evolved the hard way.

3. **Decide cross-cutting concerns first.** Networking, save, input, and persistence change
   the shape of everything. Their assumptions get documented *before* gameplay is
   implemented, not retrofitted after.

4. **Every system owns one responsibility.** Files are organized by responsibility, not by
   the order features happened to arrive. When a file needs "and" to describe it, split it.

5. **Every feature has a measurable purpose.** It supports a named design pillar and you can
   state how you'll know it worked. If it can't answer that, it isn't ready.

6. **Playtesting beats opinion.** Ship it to a real person on real hardware before you trust
   your read on it. Don't optimize or polish before playtests point at what matters.

7. **Simplicity beats cleverness.** The simplest solution that holds is the correct one.
   Every dependency and every abstraction is a debt that requires written justification.

8. **You can't fix what you can't see.** Build the ability to observe — diagnostics, capture,
   verification — *before* you need it, not in a panic mid-project.

9. **"Done" means verified in the artifact you actually ship.** Not "it compiles," not "it
   works in dev." The deployed build is authoritative.

10. **Documentation is part of the feature.** A feature isn't done until the decision behind
    it is logged and the relevant OS doc reflects reality.

11. **AI is a collaborator, not the designer.** Agents implement against this OS's intent.
    The vision, the pillars, and the taste are Wyatt's. Preserve momentum — keep the agents
    pointed at the same north star.
