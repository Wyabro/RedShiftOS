# Godot 4.7 — M1 hard cuts

*Pull this when the game’s stack is Godot. Not always-load. Engine version is **4.7.x**
(the version on this machine and in `project.godot` features). Skills and MCP below are
chosen because they name 4.7 as the baseline — not because they are popular.*

Slop Park (Game #2) used Godot 4.7.1 and still dead-ended. Godot was not the failure.
Experimental physics, a whole-game slice, and a false PASS were.

---

## Engine pin

- **Godot 4.7.x** matching the editor you actually run (4.7.1, 4.7.2, …). Do not mix snippets
  from Godot 3.x or from skills that do not name 4.7.
- **Stock Jolt** (Godot 4.6+ default). No Box3D, no custom physics GDExtension, no engine fork
  in M1 (L-19).
- **GDScript** unless `PROJECT.md` records a written C# decision. If C#, you need a Godot
  **.NET 4.7** editor, not a random `dotnet` skill.
- One authored lot / scene for prove. No generated 800 m world.

## If 1.0 is multiplayer (L-22)

Day 0 in `PROJECT.md`: player count + authority (default: host-authoritative).

Prove on **two local inputs** (two pads, two keyboard maps, or two clients on `127.0.0.1`)
before content grows. Use Godot 4.7 high-level multiplayer (`ENetMultiplayerPeer`, `@rpc`,
`MultiplayerSpawner` / `MultiplayerSynchronizer`) — not a Steam page and not a 20-decision
net novel.

Steam / GodotSteam waits until KEEP.

## Agent loop on this engine

Follow [`AI/Agent-Loops.md`](../../AI/Agent-Loops.md). Godot-specific floor:

```text
<godot-4.7-console.exe> --headless --path . --import
<godot-4.7-console.exe> --headless --path . --quit-after 10
```

Fail on `SCRIPT ERROR`. Windowed runs are for screenshots you will look at, not for logic
logs (Slop Park: Start-Process ate prints).

### MCP candidate (evaluate on this Windows box, then pin in the game)

**[`regiellis/godot-mcp-go`](https://github.com/regiellis/godot-mcp-go)** — MIT. Developed
and released against **Godot 4.7**. Runtime input, screenshots, errors, breakpoints, CLI
first.

Pin a **release `.exe`**. Do not make “install Go 1.26 and build” a day-0 tax.

Hard rules if adopted:

1. `godot-mcp serve --typed=false` (or CLI only). Default typed MCP is ~52k tokens.
2. Run `godot-mcp doctor` after install. Releases 0.6.0–0.8.2 omitted game autoloads.
3. Disable the addon and exclude `addons/godot_mcp/*` from **every** export preset.
4. MCP output is TECH PASS / observation. Never HUMAN PASS.

Fallback if it is sick: headless floor above. Do not swap in a second 300-tool MCP “while
you’re here.”

`hi-godot/godot-ai` is editor-strong and 4.5+; it is not the 4.7-targeted runtime loop we
want first. Revisit only if mcp-go fails the doctor on this machine.

## Skills allow-list (Godot 4.7 only)

Source: [`gamedev-skills/awesome-gamedev-agent-skills`](https://github.com/gamedev-skills/awesome-gamedev-agent-skills)
(Apache-2.0). Version table (checked 2026-08-28): **Godot 4.7** for new projects.

These are approved candidates, not a default install. Copy a skill into the **game** repo
(`.agents/skills/` / `.claude/skills/`), never into RedShiftOS, only when the current task
matches its trigger. Before copying it, open its `SKILL.md` and confirm it still targets
Godot 4.7 when it is engine-specific.

| Skill | When |
|---|---|
| `prototype-fast` | Concept prove (engine-agnostic workflow; still 4.7 when the engine skill loads) |
| `godot-gdscript` | Any GDScript |
| `godot-nodes-scenes` | Scene work |
| `godot-physics` | Bodies, layers, raycasts — stock 4.7 physics |
| `godot-3d-essentials` | If the prove is 3D |
| `godot-2d-movement` | If the prove is 2D |
| `godot-multiplayer` | If `PROJECT.md` is co-op — 4.7 ENet / `@rpc` |
| `input-systems` | From first prove: named actions, edge/held input, deadzones, and required devices. Rebinding UI and saved bindings wait until KEEP |
| `physics-tuning` | After the verb exists |
| `game-feel` | After KEEP on the verb, before polish spirals |
| `godot-signals-groups` | After KEEP, when production scenes need decoupled communication |
| `camera-systems` | During prove only when camera behavior is the question; otherwise after KEEP |
| `godot-ui-control` | After KEEP, for HUD, menus, layout, themes, and controller focus |
| `godot-export` | First external human build and every release/QA build |

Keep the upstream Apache-2.0 license and any supplied notice with copied skills. Add their
source and version to the game's third-party provenance record in the same commit.

**Do not install:** the upstream master `router`; the other 50+ skills; Unity/Unreal/web
packs; `game-jam` (overlaps `prototype-fast`); genre packs before the concept is chosen;
`create-game-assets` during prove (player-facing gen art); `steam-publish` before its release
advice is checked against current official Steamworks documentation; or any Godot collection
that does not pin **4.7** (unversioned “Godot 4.x” mega-packs, Godot 3 snippets,
GD-Agentic-Skills as a whole dump).

Audio, animation, resources, save, level-design, and performance skills stay task-dependent.
Re-evaluate them after KEEP instead of expanding the day-0 skill surface.

GUT / GdUnit4: after KEEP. They test behavior, not fun.

## Reuse — Store first, then Kenney, then GitHub

Do not rewrite a solved Godot problem when a **4.7** addon or a CC0 kit already does it.
Pull [`LIFECYCLE/6-PRODUCTION/Assets-and-Provenance.md`](../6-PRODUCTION/Assets-and-Provenance.md)
— license bucket, then version, then code.

On this engine:

1. **Code / addons:** [Godot Asset Store](https://store.godotengine.org) (4.7 in-editor). Fall
   back to the [Asset Library](https://godotengine.org/asset-library/asset) only if it is not
   on the Store yet. Then GitHub `godot-addon` with a `LICENSE`.
2. **Greybox art / UI / SFX:** [Kenney](https://kenney.nl) (CC0). HDRIs/textures:
   [Poly Haven](https://polyhaven.com) (CC0). Shaders: [godotshaders.com](https://godotshaders.com)
   (per-page license).
3. Open the listing **and** the `LICENSE`. If they disagree, skip.
4. Prefer Godot **4.7**. **4.x** needs a 4.7.x smoke + tweak note. Godot 3: no.
5. Copy `LICENSE` next to the files. Fill the third-party table in the **same commit**.
6. **GPL/AGPL code does not ship** unless Wyatt accepts that in writing. CC-BY-NC does not
   ship on Steam.
7. Prove: at most one permissive addon, and only if it is the core verb. Kenney CC0 kits do
   not count as that addon. No Box3D-class physics plugins (L-19).
8. Export presets exclude editor-only addons.

An agent does not bulk-install the Store.

## Export / production hygiene

- Dev addons off in export presets.
- Headless CI uses the same 4.7.x patch as the editor.
- Human-made (or human-reworked) player-facing assets — see
  [`LIFECYCLE/6-PRODUCTION/Assets-and-Provenance.md`](../6-PRODUCTION/Assets-and-Provenance.md).
