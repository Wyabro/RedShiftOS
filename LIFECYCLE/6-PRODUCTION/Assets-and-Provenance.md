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

| Path | Source URL | License | Engine tag | Tweaks (4.7) | Ships? | Attribution owed |
|---|---|---|---|---|---|---|
| `addons/example/` | https://store.godotengine.org/asset/… | MIT | 4.7 | none | editor-only | LICENSE kept in addon folder |

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

- **During prove:** only if it unblocks the **one question**. Stock nodes beat a plugin.
  No physics-replacing GDExtension (L-19). No GPL. One addon, not a shopping spree.
- **After KEEP:** still one justification per addon in `decisions.md` or this log. Disable
  editor-only addons in export presets (same rule as the MCP addon).

### What “documented” means

The third-party table row is filled **in the same commit** as the files. The commit names the
addon. Do not `git add -A` a mystery `addons/` tree. If you fork or heavily tweak, the
**Tweaks** cell says what changed and that upstream LICENSE still applies.

## Provenance in one paragraph

Raw AI output is generally not copyrightable on its own; **human creative editing and
arrangement** is what creates protectable work. So: record what the human actually changed,
keep internal drafts separate from shipped assets, and note in the log which assets are
own-work, AI-assisted, or third-party (with license). When in doubt on a *final, shipped*
asset, get it human-made or human-reworked enough to stand on its own.
