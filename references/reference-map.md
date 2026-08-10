# Reference map

Use this file to route a task to primary evidence. Read `references/provenance.md` before attributing a bundled file to an upstream version. Treat every bundled directory as a snapshot; no bundled file records a guaranteed match to the caller's live server unless the caller confirms it.

## Bundled SourceMod headers

| Path | Use |
| --- | --- |
| `references/sm-1.11/include/` | Check SourceMod 1.11 declarations and compile-time availability. |
| `references/sm-1.12/include/` | Check SourceMod 1.12 declarations and compile-time availability. |

Read `version.inc` in the selected tree. These copies identify themselves as `1.11.0-manual` and `1.12.0-manual`; `references/provenance.md` records the verified source revisions, but neither snapshot proves the caller's live server build.

## Bundled dependency headers

| Path | Use |
| --- | --- |
| `references/left4dhooks/include/` | Verify Left4DHooks natives, forwards, enums, stocks, and their documentation comments. |
| `references/left4dhooks/data/left4dhooks.l4d2.cfg` | Verify the bundled L4D2 animation/activity configuration used by the matching Left4DHooks snapshot. |
| `references/left4dhooks/gamedata/left4dhooks.l4d2.txt` | Inspect signatures, offsets, detours, and SDKCalls for the matching Left4DHooks snapshot. |
| `references/left4dhooks/gamedata/lux_library.txt` | Inspect gamedata loaded directly by `left4dhooks_lux_library.inc` stocks. |
| `references/l4d2util/scripting/include/` | Maintain code already using this unversioned local l4d2util fork; consult provenance before attributing helpers to upstream. |
| `references/multicolors/scripting/include/` | Verify multicolors declarations already used by a project. |
| `references/standalone/colors.inc` | Verify standalone `colors.inc` declarations already used by a project. |
| `references/smlib/scripting/include/` | Maintain existing SMLib-dependent code; `smlib/colors.inc` is a local variant of unknown origin. Do not introduce SMLib into new code without a project requirement. |
| `references/smlib/gamedata/smlib_colors.games.txt` | Check optional overrides used by the bundled SMLib colors variant. |

Search the exact symbol and read surrounding comments and types:

```text
rg -n -C 8 "ExactSymbol" references/left4dhooks/include references/l4d2util/scripting/include
rg -n "enum|typeset|methodmap|native|forward|stock" <matching-file>
```

## Bundled L4D2 data

| Path | Use |
| --- | --- |
| `references/game-data/l4d2-events.txt` | Verify event names, field names, and field types in the bundled MediaWiki-formatted snapshot. |
| `references/game-data/DirectorOptions.txt` | Verify Director script keys and nearby documentation. |
| `references/game-data/netprop-dump.txt` | Discover send properties for the captured dump. |
| `references/game-data/datamap-dump.txt` | Discover datamap fields for the captured dump. |

Do not treat numeric offsets in the dumps or bundled gamedata as portable across game builds. Prefer named SourceMod property access. For signatures, addresses, or build-specific offsets, require gamedata that matches the target server.

## Current upstream sources

Use upstream sources when the user asks for current behavior, the bundled snapshot is insufficient, or the target environment differs. Prefer code and generated API documentation over summaries.

- SourceMod API: <https://www.sourcemod.net/new-api/>
- SourceMod source: <https://github.com/alliedmodders/sourcemod>
- SourcePawn source: <https://github.com/alliedmodders/sourcepawn>
- SourceMod scripting guides: <https://wiki.alliedmods.net/Category:SourceMod_Scripting>
- Left4DHooks source: <https://github.com/SilvDev/Left4DHooks>
- Left4DHooks maintainer thread: <https://forums.alliedmods.net/showthread.php?t=321696>
- l4d2util source: <https://github.com/ConfoglTeam/l4d2util>
- Multi-Colors source: <https://github.com/Bara/Multi-Colors>
- SMLib source: <https://github.com/bcserv/smlib>

Do not assume current upstream declarations exist on the target server. Record the source URL or revision used and reconcile it with the target compiler, include files, plugin versions, and gamedata.

The public SourceMod web compiler accepts source uploads and currently targets SourceMod 1.12 or higher. Do not upload user code without explicit approval; prefer the user's local compiler.
