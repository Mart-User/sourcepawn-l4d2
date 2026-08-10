---
name: sourcepawn-l4d2
description: Develop, review, debug, and compile SourcePawn plugins for Left 4 Dead 2 with SourceMod. Use for .sp/.inc code, SourceMod 1.11/1.12 compatibility, Left4DHooks or l4d2util APIs, L4D2 game events, DirectorOptions, netprops, datamaps, entity lifetime, and compiler or runtime diagnostics. Do not use for VScript/Squirrel, client-side addons, or unrelated Source games unless the task also changes L4D2 SourceMod code.
---

# SourcePawn for Left 4 Dead 2

Produce the smallest correct change for the caller's actual L4D2 server environment. Ground every API signature, event field, constant, and property name in a matching include, dump, compiler result, or upstream source. Never invent missing details.

## Follow the workflow

### 1. Establish the target

1. Inspect the relevant `.sp`, `.inc`, build scripts, compiler output, and dependency files before editing.
2. Identify the target SourceMod version, Left4DHooks/l4d2util requirements, and server OS when gamedata or paths make the OS relevant.
3. Preserve the file's existing declaration syntax and style. Do not migrate old declarations or add broad boilerplate unless requested.
4. If an unknown target version materially changes the implementation, ask for it. When practical, verify against both bundled SourceMod header sets instead of guessing.
5. State a concrete success condition, such as reproducing a compiler error and making the same command pass without warnings.

### 2. Load only the required evidence

Read [references/reference-map.md](references/reference-map.md), consult [references/provenance.md](references/provenance.md) before attributing a bundled dependency to upstream, then open only the files needed for the task.

- Search declarations by exact symbol. Read the declaration, its documentation comment, related enum/typeset/methodmap, and any version guards around it.
- Search event names and fields in `references/game-data/l4d2-events.txt`; preserve the recorded field type and distinguish user IDs from entity indexes.
- Search both netprop and datamap dumps when the property kind is uncertain. Confirm the owning entity/class before choosing `Prop_Send` or `Prop_Data`.
- Search `DirectorOptions.txt` for the exact key and its surrounding context. Do not infer a key from a similarly named ConVar.
- Treat bundled files as snapshots. If the caller asks for current behavior, a local snapshot lacks the symbol, or a server log shows a different build, consult the official/upstream sources listed in the reference map.
- Record which version or snapshot supports the answer. Do not silently combine a target server's old headers with a newer upstream declaration.

Prefer targeted searches, for example:

```text
rg -n "ExactSymbol" references/sm-1.12/include references/left4dhooks/include
rg -n -C 8 "event_name|field_name" references/game-data/l4d2-events.txt
rg -n -C 4 "m_propertyName" references/game-data/netprop-dump.txt references/game-data/datamap-dump.txt
```

### 3. Implement the minimal change

- Add only the includes and state required by the requested behavior.
- Copy callback and native signatures from the selected source exactly. Do not infer return semantics from names such as `Pre`, `Post`, or `PostHandled`; read that forward's documentation.
- Convert an event user ID with `GetClientOfUserId()` only when the field is documented as a user ID. Validate the resulting client for the operation being performed.
- Use `client > 0 && client <= MaxClients && IsClientInGame(client)` when an in-game client is required. Add team, alive, bot, or authorization checks only when the operation requires them.
- Use entity references for entities retained across timers, frames, or other deferred callbacks. Resolve the reference and validate it at use time.
- Access netprops/datamaps by verified names and the correct property type. Do not hardcode offsets copied from a dump unless the user explicitly requests build-specific low-level work and the exact target build has been verified.
- Put signatures, offsets, and address calculations that truly require raw memory access in matching gamedata rather than embedding them in plugin logic.
- Use compile-time guards only for compile-time API availability. Use `GetFeatureStatus`, `LibraryExists`, or optional-native handling when runtime dependency availability is the real concern.
- Release handles when their useful lifetime ends. Do not add redundant unload-only cleanup: SourceMod releases plugin-owned handles and hooks when a plugin unloads.
- Handle late loading only when the feature must reconstruct state or attach hooks to entities/clients that already exist.
- Keep hot callbacks such as `OnGameFrame`, damage hooks, and command listeners bounded; avoid repeated scans or allocations unless the requested behavior needs them.

### 4. Verify with the real toolchain

1. Locate the project's existing compiler command or the target SourceMod installation's `spcomp`. Do not invent machine-specific paths.
2. Compile with the compiler and include set that match the target server. Add only the required third-party include directories.
3. Re-run the original failing command when fixing a build issue. Treat new warnings as failures unless the project explicitly accepts them.
4. To claim SourceMod 1.11 and 1.12 compatibility, compile with matching toolchains/header sets for both versions. If either compiler is unavailable, state exactly what was not verified.
5. Run repository tests or a focused server smoke test when available. Compilation proves syntax and type compatibility, not gameplay behavior, entity timing, event delivery, or gamedata validity.
6. Never upload private source to the public SourceMod web compiler without explicit user approval.

Use the compiler's own help for flags because available options can differ by compiler build:

```text
spcomp --help
spcomp plugin.sp -i<sourceModInclude> -i<thirdPartyInclude> -o<outputFile>
```

### 5. Report the result

Report:

- the target versions and assumptions used;
- the exact local or upstream declarations consulted;
- the compile/test command and its result;
- any runtime behavior that still requires an L4D2 server to verify.

Do not claim runtime compatibility from a successful compile alone.

## Use the focused playbooks

### Look up an API

1. Select the target include tree.
2. Find the exact declaration and surrounding documentation.
3. Trace referenced types, constants, and required includes.
4. Confirm whether the symbol exists in every claimed target version.
5. Return the verified signature and version boundary; say when the evidence is incomplete.

### Fix a compiler error

1. Run or capture the exact failing command and diagnostic.
2. Check whether the compiler and include files come from the same SourceMod installation.
3. Classify the failure as syntax, signature, missing include, dependency mismatch, or version mismatch.
4. Make the smallest correction.
5. Recompile with the same command and confirm that no new warnings appear.

### Implement event logic

1. Verify the event name and each field in the event dump or the target game's resource files.
2. Match the hook mode to whether the task must observe or alter behavior.
3. Convert and validate identifiers according to their documented type.
4. Test disconnect, world/entity attacker, bot, round transition, and late-load cases only when they can occur in this event path.

### Work with netprops or datamaps

1. Identify the concrete entity classname and server class/datamap.
2. Confirm the property in the appropriate dump.
3. Verify scalar, entity, string, vector, or array access and its element requirements in the SourceMod include docs.
4. Prefer named access. Treat dump offsets as diagnostics for the captured build, not portable constants.

### Use Left4DHooks

1. Read `left4dhooks.inc` and any included stock file containing the symbol.
2. Follow that symbol's documented game support, parameters, return values, and pre/post behavior.
3. Check the upstream repository or maintainer release thread for current changes only when current behavior matters.
4. Confirm the target server actually has the required Left4DHooks plugin/gamedata; having an include file proves compilation only.

### Create a new plugin

1. Confirm the requested behavior and deployment target.
2. Start with `<sourcemod>` and add only the APIs actually used.
3. Reject non-L4D2 engines in `AskPluginLoad2` only when the plugin is intentionally L4D2-only.
4. Add ConVars, commands, translations, cookies, configs, or optional dependencies only when required.
5. Compile before adding secondary behavior, then perform the smallest available runtime smoke test.

## Enforce evidence boundaries

Apply this precedence order:

1. The target server's compiler, includes, gamedata, logs, and observed behavior.
2. A matching bundled snapshot under `references/`.
3. Official SourceMod documentation/source or the dependency maintainer's upstream source.
4. Maintainer release notes and forum posts.
5. Secondary examples only as leads, never as proof of a signature or field.

If authoritative evidence is missing or conflicting, stop and state the uncertainty. Ask for the target include, gamedata, compiler output, or server build instead of fabricating a value.

## Completion checklist

- Verify every changed API and field against the chosen source.
- Keep the diff limited to the requested behavior.
- Compile with the target toolchain and fix introduced warnings.
- Separate compile-time evidence from runtime evidence.
- State version assumptions and unverified server behavior.
