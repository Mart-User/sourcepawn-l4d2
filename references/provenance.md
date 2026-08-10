# Reference provenance

This audit records what can be proven about the bundled references as of 2026-08-07. A complete file-name set does not prove compatibility with a live server; match the target compiler, plugin build, and gamedata separately.

## Verification method

- Compare recursive file manifests with the maintainer repository at a fixed Git revision.
- Compare Git blob content while allowing only CRLF/LF normalization for previously bundled text files.
- Search all reachable repository history when a local variant does not match the selected revision.
- Mark a source unknown when neither the file nor its metadata proves one. Do not infer a version from syntax or similar names.

## Verified snapshots

| Local path | Maintainer source | Result |
| --- | --- | --- |
| `sm-1.11/include/` | [alliedmodders/sourcemod `f53cb134`](https://github.com/alliedmodders/sourcemod/tree/f53cb134ef83b580c83e1f4bf35f60d11c4571dd/plugins/include) | Complete 70-file manifest; all text matches after line-ending normalization. |
| `sm-1.12/include/` | [alliedmodders/sourcemod `e5de0a0c`](https://github.com/alliedmodders/sourcemod/tree/e5de0a0cdef3eb2bc484b17ce63a4e14fcf3f221/plugins/include) | Complete 70-file manifest; all text matches after line-ending normalization. |
| `left4dhooks/include/` | [SilvDev/Left4DHooks `f90ae5e6`](https://github.com/SilvDev/Left4DHooks/tree/f90ae5e62228e0b7baf12cda922e3fd40db844f4/sourcemod/scripting/include) | Complete 5-file manifest; all text matches after line-ending normalization. |
| `left4dhooks/data/left4dhooks.l4d2.cfg` | [same fixed revision](https://github.com/SilvDev/Left4DHooks/blob/f90ae5e62228e0b7baf12cda922e3fd40db844f4/sourcemod/data/left4dhooks.l4d2.cfg) | Exact Git blob `2d73bb337b8284db998c6841ea6544d4b5066b33`. |
| `left4dhooks/gamedata/left4dhooks.l4d2.txt` | [same fixed revision](https://github.com/SilvDev/Left4DHooks/blob/f90ae5e62228e0b7baf12cda922e3fd40db844f4/sourcemod/gamedata/left4dhooks.l4d2.txt) | Exact Git blob `de9848eb022df416928caad77e15e467520110bc`. |
| `left4dhooks/gamedata/lux_library.txt` | [same fixed revision](https://github.com/SilvDev/Left4DHooks/blob/f90ae5e62228e0b7baf12cda922e3fd40db844f4/sourcemod/gamedata/lux_library.txt) | Exact Git blob `dca4b6f13d636a9289ea8373d1bc69f4c17c98c9`. |
| `multicolors/scripting/include/` | [Bara/Multi-Colors `7e39d040`](https://github.com/Bara/Multi-Colors/tree/7e39d040139a8a05cdc5bf792bec40df982cda33/addons/sourcemod/scripting/include) | Complete 3-file manifest; exact Git blobs. |
| `standalone/colors.inc` | [Multi-Colors `colors.inc` at `7e39d040`](https://github.com/Bara/Multi-Colors/blob/7e39d040139a8a05cdc5bf792bec40df982cda33/addons/sourcemod/scripting/include/multicolors/colors.inc) | Exact duplicate of that upstream file. |

## Verified local variants

| Local path | What is proven | Required treatment |
| --- | --- | --- |
| `l4d2util/scripting/include/` | The set is self-contained with 8 headers, but none of those file blobs occurs anywhere in the reachable history of [ConfoglTeam/l4d2util](https://github.com/ConfoglTeam/l4d2util). Its extra `l4d2util_stocks.inc` is not in upstream `master` at `618e922bbd09229c0ad882e7bfd18cfec8b23a4f`. | Treat it as an unversioned local fork. Do not attribute its added helpers or signatures to ConfoglTeam without another source. |
| `smlib/scripting/include/` | The 24-file manifest matches branch `transitional_syntax` at `156acfd35e777015e37ae3fad9c048ab2c45fe79`; 23 files match that branch after line-ending normalization. `smlib/colors.inc` does not occur as a blob anywhere in reachable [bcserv/smlib](https://github.com/bcserv/smlib) history. | Treat only `smlib/colors.inc` as a local modification of unknown origin. Do not call the whole tree release `0.9.7` merely because `smlib.inc` contains that macro. |
| `smlib/gamedata/smlib_colors.games.txt` | Its blob occurs in the official bcserv/smlib history. | Use only with the bundled SMLib colors variant; the file comments say distribution is optional. |

## Game-data snapshots without complete capture metadata

- `game-data/l4d2-events.txt` is MediaWiki source corresponding to the [AlliedModders L4D2 events page, revision 11395](https://wiki.alliedmods.net/index.php?title=Left_4_Dead_2_Events&oldid=11395). The local file does not embed a capture date.
- `game-data/DirectorOptions.txt` is MediaWiki-formatted DirectorOptions documentation. The local file does not embed a page revision or capture date, so use the [current Valve Developer Community page](https://developer.valvesoftware.com/wiki/Left_4_Dead_2/Scripting/Expanded_Mutation_System/Appendix:_DirectorOptions) for current claims.
- `game-data/netprop-dump.txt` and `game-data/datamap-dump.txt` do not identify the L4D2 build, server OS, SourceMod build, command invocation, or capture date. Their names and offsets are useful for discovery only; their numeric offsets are not attributable to a current game build.

## Scope decision

The reference bundle intentionally does not vendor compiled plugins, L4D1-only files, test sources, or full dependency implementations. For current implementation behavior, inspect the fixed upstream source or the target server package instead of expanding this skill with unrelated files.
