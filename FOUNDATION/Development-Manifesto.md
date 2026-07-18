# Development Manifesto

*How Red Shift Studios builds games. Rules on the wall — not a pep talk. Every line is something you
can be held to. If it grows past two pages it's a rulebook; cut it back.*

> **Voice locked (rules-on-the-wall). Now gut-check the beliefs.** These are seeded from how
> Cart Clash actually got built. Delete any rule you don't hold, sharpen the wording, add the
> ones only you'd write. A manifesto you don't believe won't get followed.

---

1. **Prove it before you build it.** Programmer art, throwaway code. If the loop isn't fun
   rough, it won't be fun polished — and no mechanic earns production structure until a
   prototype proves it's worth keeping. Requirements → prototype → validate → *then* build it
   for real. This is where Cart Clash bled the most time.

2. **Playtesting beats opinion.** Put it in front of a real person on real hardware before
   you trust your read. Don't polish or optimize until a playtest tells you what matters.

3. **Nail the cross-cutting stuff before gameplay.** Networking, save, input, time base —
   they touch everything. Decide them early, even if they ship later. Retrofitting them is
   the most expensive mistake you can make. (It's the one you already made.)

4. **One responsibility per file.** Organize by what a thing *does*, not by the order
   features showed up. If describing a file needs the word "and," split it.

5. **Simplicity beats cleverness.** The simplest thing that holds is the right thing. Every
   dependency and every abstraction is debt — make it justify itself in writing, or cut it.

6. **Every feature earns its place.** It should serve a pillar or make the game more fun. If
   you can't say what it's for, it's probably not worth building — cut what doesn't pull its
   weight.

7. **Build the ability to see before you need it.** Capture, diagnostics, a headless harness
   — early, not mid-crisis. You can't fix what you can't watch.

8. **"Done" means verified in the build you ship.** Not "it compiles." Not "works in dev."
   Pull the deployed artifact, confirm the build stamp, watch it work. Remote is the truth.

9. **Write down the decisions that matter.** When a choice is big or non-obvious, log what
   you picked, what you passed on, and why. Six months out, "why did I do this?" should have
   an answer — for you and for the agents.

10. **AI builds; you direct.** Agents implement against this OS. The vision, the pillars, the
    taste — those stay yours. Keep every agent pointed at the same north star. Keep momentum.
