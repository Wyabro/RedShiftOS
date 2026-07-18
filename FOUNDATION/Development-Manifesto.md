# Development Manifesto

*How RedShift builds games. Rules on the wall — not a pep talk. Every line is something you
can be held to. If it grows past two pages it's a rulebook; cut it back.*

> **Voice locked (rules-on-the-wall). Now gut-check the beliefs.** These are seeded from how
> Cart Clash actually got built. Delete any rule you don't hold, sharpen the wording, add the
> ones only you'd write. A manifesto you don't believe won't get followed.

---

1. **Prove the loop before you build the game.** Programmer art only. If it's not fun rough,
   it won't be fun polished — stop and fix the loop before anything else gets made.

2. **Prototype before you architect.** No production structure until a throwaway version
   earns it. Requirements → prototype → validate → *then* build it for real. This is where
   Cart Clash bled the most time.

3. **Playtesting beats opinion.** Put it in front of a real person on real hardware before
   you trust your read. Don't polish or optimize until a playtest tells you what matters.

4. **Nail the cross-cutting stuff before gameplay.** Networking, save, input, time base —
   they touch everything. Decide them early, even if they ship later. Retrofitting them is
   the most expensive mistake you can make. (It's the one you already made.)

5. **One responsibility per file.** Organize by what a thing *does*, not by the order
   features showed up. If describing a file needs the word "and," split it.

6. **Simplicity beats cleverness.** The simplest thing that holds is the right thing. Every
   dependency and every abstraction is debt — make it justify itself in writing, or cut it.

7. **Every feature earns its place.** It serves a named pillar and you know how you'll judge
   it — playtest feel counts as a metric. Can't say what it's for? It's not ready.

8. **Build the ability to see before you need it.** Capture, diagnostics, a headless harness
   — early, not mid-crisis. You can't fix what you can't watch.

9. **"Done" means verified in the build you ship.** Not "it compiles." Not "works in dev."
   Pull the deployed artifact, confirm the build stamp, watch it work. Remote is the truth.

10. **Log the decision or it didn't happen.** Six months out, "why did I do this?" needs an
    answer. Write down what you chose, what you rejected, and why — for you and for the agents.

11. **AI builds; you direct.** Agents implement against this OS. The vision, the pillars, the
    taste — those stay yours. Keep every agent pointed at the same north star. Keep momentum.
