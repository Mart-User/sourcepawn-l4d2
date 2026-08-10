# sourcepawn-l4d2

An Agent Skill for developing, reviewing, debugging, and compiling Left 4 Dead 2 SourceMod plugins.

The Skill uses progressive disclosure: `SKILL.md` contains the source-first workflow, while exact SourceMod, Left4DHooks, l4d2util, event, DirectorOptions, netprop, and datamap details remain in `references/` and are loaded only when needed.

## Install for Codex

Copy this repository so the installed Skill file is located at one of these paths:

```text
$HOME/.agents/skills/sourcepawn-l4d2/SKILL.md
<repository>/.agents/skills/sourcepawn-l4d2/SKILL.md
```

Keep the `references/` and `agents/` directories beside `SKILL.md`. The installed parent directory must be named `sourcepawn-l4d2` to match the Skill name.

## Contents

- `SKILL.md`: concise execution and verification workflow
- `references/reference-map.md`: routing and upstream source policy
- `references/provenance.md`: fixed revisions, completeness checks, and explicitly unverified local variants
- `references/sm-1.11/include/`: bundled SourceMod 1.11 header snapshot
- `references/sm-1.12/include/`: bundled SourceMod 1.12 header snapshot
- `references/left4dhooks/`: bundled Left4DHooks headers plus matching L4D2 data and gamedata
- `references/l4d2util/`, `multicolors/`, `smlib/`, and `standalone/`: dependency header snapshots
- `references/game-data/`: L4D2 event, DirectorOptions, netprop, and datamap snapshots
- `agents/openai.yaml`: Codex/ChatGPT UI metadata

Bundled files are reference snapshots, not proof of a live server's exact build. The Skill requires the agent to reconcile them with the target compiler, includes, gamedata, logs, and upstream sources before claiming compatibility.

## Primary upstream sources

- [SourceMod API](https://www.sourcemod.net/new-api/)
- [SourceMod source](https://github.com/alliedmodders/sourcemod)
- [SourcePawn source](https://github.com/alliedmodders/sourcepawn)
- [Left4DHooks source](https://github.com/SilvDev/Left4DHooks)
- [l4d2util source](https://github.com/ConfoglTeam/l4d2util)

## License

See `LICENSE`. Bundled third-party reference files retain their upstream notices and license terms.
