# Assets & Provenance

*Keep an asset log from day one. Retrofitting it later — once there are hundreds of generated
and hand-made files — is painful. This is production hygiene, not legal advice.*

---

## The asset log

Two tables in the **game** repo (start them the day the first file lands). Retrofitting
hundreds of Asset Library addons is the expensive version of this job.

**Own / AI-assisted work:**

| Asset | Tool / source | Human edits | Final use | Risk notes |
|---|---|---|---|---|
| `crowd_loop.opus` | recorded + DAW chain | trimmed, EQ, loopified | Cart Rave bed | own recording |
| `sunglasses_env` | AI draft → hand-tuned gradient | recolored to reference | cart cosmetic | AI-assisted |

**Third-party (Asset Store, Asset Library, Kenney, GitHub, packs):**

| Path | Source URL | Version / commit | License | Engine tag | 4.7 smoke / tweaks | Ships? | Attribution owed |
|---|---|---|---|---|---|---|---|
| `addons/example/` | https://store.godotengine.org/asset/… | `v1.2.3` | MIT | 4.7 | PASS: intended seam loads; none | editor-only | LICENSE kept in addon folder |

## Where AI is safe vs. risky

- **Use AI freely** for drafts, placeholders, moodboards, programmer-art stand-ins — anything
  you'll replace or heavily rework.
- **Be careful with *final*** logos, character designs, voice likenesses, and music. These are
  the assets that carry brand and legal weight.

## Steam disclosure (2026 wording — not legal advice)

Valve’s Content Survey targets **AI-created material that ships and is consumed by players**
(artwork, sound, narrative, localization, marketing). Ordinary **efficiency tools** (coding
agents, Copilot) are not the focus.

Studio default:

- AI may write **code** and greybox placeholders.
- Player-facing art, animation, writing, audio, localization, and store assets are
  **human-made or human-reworked** enough to stand on their own.
- Do not describe this as “hiding AI.” Build a pipeline with no player-consumed generated
  material to hide.
- Answer the survey truthfully. Confirm with Steamworks Support before store review if the
  wording still feels broad.
- Keep this asset log regardless.

Greybox / programmer-art in a prove does not go to the store.

## Third-party sources (borrow, don't reinvent)

Prefer an existing addon or CC0 kit over a from-scratch rewrite when it answers a real need
(Manifesto §5). **License first, then version, then code.** This is not legal advice.

### Asset value and visual fit

Name the asset gap before shopping. A large bundle is not automatically valuable. Inspect one
representative asset at the game's camera distance and intended scale before bulk import. Prefer
editable source formats, clear commercial and modification rights, and a source format the game
can own through a narrow adapter. Use a source-specific adapter before designing a universal
import framework. Keep private paid sources outside public repositories. Record provenance in the
same change as derived runtime assets.

For a first-person game, judge the model at player-eye distance. A top-down or diorama pack may
be useful greybox material while failing as production art.

### Where to look (Godot 4.7)

**Do now** — same log + license buckets for all of these:

| Source | URL | What it's for |
|---|---|---|
| **Godot Asset Store** | https://store.godotengine.org | Official 4.7 in-editor store. Addons, templates, tools. **First stop for code.** The old Asset Library is deprecated and will go read-only; listings were not auto-migrated. |
| **Asset Library** (legacy) | https://godotengine.org/asset-library/asset | Still useful while authors have not re-uploaded. Same license rules. |
| **Kenney** | https://kenney.nl | CC0 greybox kits, UI, audio. Fastest prove art. Human-made, not gen-AI. Log the pack name. |
| **Poly Haven** | https://polyhaven.com | CC0 HDRIs, textures, some models. |
| **Godot Shaders** | https://godotshaders.com | One shader at a time. Read **that** page's license (often CC0/MIT). |
| **GitHub `godot-addon`** | search the topic | Same as Store/Library, often newer. Repo must have `LICENSE`. Smoke on 4.7.x. |

**Useful later** (filter licenses hard; many NC / “personal use”):

- itch.io Godot pages
- Quaternius / KayKit (usually CC0 character/prop kits)

**Skip or high-risk:**

- Unity Asset Store (wrong engine, paid EULA)
- Mixamo (Adobe terms — not a Steam default)
- Random “free Kenney” mirrors (ownership is unclear; use kenney.nl)
- BlenderKit→Godot **plugins** that are GPL even when the models are commercial-OK
- OpenGameArt / Freesound dumps without reading each file (lots of NC and SA)

### License buckets (read the listing *and* the repo `LICENSE`)

Licenses are not interchangeable. Listing license must match the repo. Keep a copy of
`LICENSE` (or the pack's license text) next to the files so it survives export hygiene.

| Bucket | Examples | Steam / commercial game |
|---|---|---|
| **Permissive** | MIT, BSD, Apache-2.0, ISC, Boost, Unlicense, CC0 | Use. Keep LICENSE. Attribute if the license asks (MIT does). |
| **Attribution** | CC-BY | Use. Credit in the shipped credits / license screen. |
| **Share-alike on that work** | CC-BY-SA | Use the asset only if you can share *that* adaptation under SA. Does not GPL the whole game. Credit. |
| **Non-commercial** | CC-BY-NC, CC-BY-NC-SA | **No.** A paid Steam page is commercial. |
| **Copyleft code** | GPL-2/3, AGPL | **Do not ship** in the game without a written, human-accepted decision. GPL GDScript/GDExtension that you distribute with the game can force the game under GPL. Editor-only tools you never export are a narrower case — still log them. |
| **Weak copyleft** | LGPL, MPL | Possible if you meet the license (dynamic link / file-level). Write the plan before install. |
| **Missing / “all rights reserved” / listing ≠ repo** | — | **Do not use.** |

Tweaking a MIT/BSD addon is allowed under that license; you still keep copyright notices.
Tweaking a GPL addon does not wash the GPL off.

Godot Engine itself is MIT. Your game may be any license. Third-party rows are extra.

### Engine tag: 4.7 first, 4.x with a tweak note

- **Prefer** assets that name **Godot 4.7** (or 4.7.x).
- **4.x / 4.3–4.6** is allowed if: (1) license is in an allowed bucket, (2) you smoke it on
  **this** 4.7.x editor (`SCRIPT ERROR` = fail), (3) you log every API tweak (3.x `yield` →
  `await`, `export` → `@export`, old `TileMap`, etc.).
- **Godot 3** listings: do not install. Port only as a named task with KEEP-level need.

### Prove vs after KEEP

- **During prove:** use only code that directly accelerates the **one player-facing question**.
  Multiple existing solutions are allowed; optimize for speed-to-learning, Godot 4.7
  compatibility, inspectability, and easy removal — not dependency count. Do not bulk-install
  addons or add speculative systems.
- **Approval lanes:** low-risk, single-purpose, permissively licensed GDScript addons may be
  approved as one named batch. Every addon still needs its own justification, pinned version
  or commit, license check, focused smoke test, and provenance row. Large frameworks or addons
  that own input, core gameplay, multiplayer authority, networking, saves, the scene tree, or
  another broad game boundary require separate explicit human approval. Native extensions are
  prohibited during prove; after KEEP, they require separate explicit human approval (L-19).
- **Ownership:** reuse solved supporting systems such as controllers, camera effects, particles,
  audio helpers, ragdolls, pooling, settings, and UI. Keep game-specific mechanics and multiplayer
  authority under the game's ownership. An existing solution may sit behind a game-owned seam;
  transferring ownership is a high-risk decision.
- **Reject:** missing or conflicting licenses, incompatible Godot versions, GPL/AGPL shipping
  code without written human acceptance, every native extension during prove, unnecessary native
  extensions after KEEP, and physics-replacing extensions during prove remain rejected.
- **After KEEP:** apply the same dependency gate. The Production Foundation Gate decides lasting
  ownership and seams before production feature work begins. Disable editor-only addons in
  export presets (same rule as the MCP addon).

### What “documented” means

The third-party table row is filled **in the same commit** as the files. Record the exact version
or commit, and name the addon in the commit. Do not `git add -A` a mystery `addons/` tree. If
you fork or heavily tweak, the **4.7 smoke / tweaks** cell names the focused scenario and result,
then says what changed and that upstream LICENSE still applies. A named same-commit evidence note
may hold the full command and output.

## Provenance in one paragraph

Raw AI output is generally not copyrightable on its own; **human creative editing and
arrangement** is what creates protectable work. So: record what the human actually changed,
keep internal drafts separate from shipped assets, and note in the log which assets are
own-work, AI-assisted, or third-party (with license). When in doubt on a *final, shipped*
asset, get it human-made or human-reworked enough to stand on its own.
