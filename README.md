# sourcepawn-l4d2

SourcePawn development skill for Left 4 Dead 2 SourceMod plugins. Provides comprehensive offline reference for SM 1.11/1.12, Left4DHooks, game events, DirectorOptions, netprops, and datamaps — with full compatibility guidance for both SourceMod versions.

## Installation

```bash
npx skills add <repo-url> --skill sourcepawn-l4d2
```

Or manually:
```bash
cp -r sourcepawn-l4d2 ~/.claude/skills/
```

## What's Included

### SKILL.md (2188 lines)
Complete reference covering:

- **SM 1.11 → 1.12 full change log** — 5 breaking changes, 14 new natives, 6 new methodmaps/types, 16 parameter changes, all with version-guarded compat code
- **Left4DHooks full API** — 100+ forwards (Pre/Post/PostHandled) and 200+ natives organized by category
- **l4d2util constants** — teams, infected classes, weapon IDs, character names, hit groups, spawn flags
- **Colored chat** — colors.inc and multicolors API with L4D2 color profile
- **Game events** — 220+ events with complete field definitions
- **DirectorOptions** — all 95 keys across General/Spawning/EMS/Finale/Gauntlet/Versus/Scavenge/Mutation categories, plus all enumerations
- **Netprop tables** — CTerrorPlayer, Infected, Tank, Witch, CTerrorPlayerResource with offsets and bit sizes
- **Datamap reference** — CBaseEntity full property map with offsets and flags
- **SM official API** — complete signatures for sourcemod/clients/entity/halflife/sdktools/events/console/timers/files/convars/sound
- **Plugin template** and **10 design rules + 10 anti-patterns**

### references/ (186 files, ~5 MB)

| Directory | Contents |
|-----------|----------|
| `sm-1.11/include/` | SM 1.11 complete API (70 .inc files) |
| `sm-1.12/include/` | SM 1.12 complete API (70 .inc files) |
| `left4dhooks/include/` | Left4DHooks extension (5 .inc files) |
| `l4d2util/scripting/include/` | l4d2util library (8 .inc files) |
| `multicolors/scripting/include/` | multicolors library (3 .inc files) |
| `smlib/scripting/include/` | SMLib transitional syntax (27 .inc files) |
| `standalone/` | colors.inc standalone include |
| `game-data/` | DirectorOptions.txt, event catalog, netprop dump, datamap dump |

## Usage

The skill triggers automatically when you work with `.sp` or `.inc` files, or when you ask about L4D2 SourceMod development. The `SKILL.md` body loads into context; the `references/` directory is available for deep dives on demand.

```sourcepawn
// With this skill active, Claude knows:
#include <left4dhooks>
#include <l4d2util>
#include <colors>

// SM version compatibility
#if SOURCEMOD_V_MINOR >= 12
    // 1.12+ path
#endif
```

## Requirements

- Claude Code
- SourceMod 1.11 or 1.12
- Left4DHooks extension (for Left4DHooks natives/forwards)
- l4d2util (optional, for convenience constants)

## Package Structure

```
sourcepawn-l4d2/
├── README.md
├── SKILL.md
└── references/
    ├── sm-1.11/include/        (70 .inc)
    ├── sm-1.12/include/        (70 .inc)
    ├── left4dhooks/include/    (5 .inc)
    ├── l4d2util/...            (8 .inc)
    ├── multicolors/...         (3 .inc)
    ├── smlib/...               (27 .inc)
    ├── standalone/colors.inc
    └── game-data/              (4 reference files)
```

## License

Reference data sourced from publicly available SourceMod SDK, Left4DHooks, l4d2util, multicolors, and SMLib. Respect the licenses of each original project.
