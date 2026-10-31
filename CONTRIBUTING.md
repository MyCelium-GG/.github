# Contributing to MyCelium

Thanks for helping. MyCelium is a family of small Minecraft mods that each do
one job, built on one shared library, [Mycel](https://github.com/MyCelium-GG/MyCel-Lib).
Every repository in [MyCelium-GG](https://github.com/MyCelium-GG) follows this guide.

## Reporting a bug

1. Check the [troubleshooting page](https://wiki.mycelium-gg.org/troubleshooting) first.
2. In game (or the server console), run `/mycel dump`, then `/mycel dump confirm`.
   You get a `dumps.mycelium-gg.org` link with your versions, installed mods,
   MyCelium configs and the end of the log. IP addresses and tokens are removed first.
3. Open an issue on the mod's repository with:
   - what you did, what you expected, and what happened instead;
   - the dump link;
   - whether it happens with only Mycel and that mod installed.

Questions and ideas are welcome on [Discord](https://discord.gg/UvQNsKWMp3) too.

## Suggesting a feature

Open an issue describing the problem you want solved, not only the solution.
Each mod does one job on purpose, so a feature may belong in a new mod instead.
That's a fine outcome.

## Changing code

You need JDK 25. Every repository builds the same way:

```sh
./gradlew build            # all three loaders + tests
./gradlew :common:test     # unit tests only
```

The first build downloads and sets up Minecraft (a few minutes, several GB of
cache). Run the game with `./gradlew :fabric:runClient` (or `:neoforge:`,
`:forge:`; `runServer` for a server).

### Layout

```
common/     the mod itself: vanilla and Mycel imports only
fabric/     Fabric entrypoint and fabric.mod.json
forge/      Forge entrypoint and mods.toml
neoforge/   NeoForge entrypoint and neoforge.mods.toml
```

### House rules

- Loader-specific code stays in the thin loader modules. Everything else goes
  in `common` and must work on Fabric, Forge and NeoForge alike.
- Use Mycel instead of rolling your own: `MycelConfig` for settings,
  `MycelDataFile` for saved data, `MycelNetwork` for packets, `TextUtil` /
  `ChatVoice` for chat. The [developer guide](https://wiki.mycelium-gg.org/developing)
  and [API docs](https://api.mycelium-gg.org) cover them.
- No per-tick scans without a reason, no blocking network calls, no required
  background threads.
- Chat colors come from `TextUtil`; never `ChatFormatting` colors or `§` codes.
- Match the code around you: naming, comment density, formatting.
- Add or update tests in `common/src/test` for logic that can run without the game.

### Pull requests

1. Fork the repository and branch from `main`.
2. Keep a pull request to one change, and describe what it does and how you tested it.
3. Add a line under `## [Unreleased]` in `CHANGELOG.md` (create the section if
   it is missing). Write it for players: what changes for them, not how.
4. Make sure `./gradlew build` passes. CI runs it on every pull request; for
   Mycel it also starts a dev server and client on all three loaders
   (`scripts/smoke.sh`).

Maintainers handle versions, tags and releases. A merged change ships with the
next release of that mod.

By contributing you agree that your contribution is licensed under the
repository's license (MIT).

## Documentation

The wiki lives in [MyCelium-Web](https://github.com/MyCelium-GG/MyCelium-Web)
(private for now). Spotted a mistake? Open an issue on the mod's repository or
say it on Discord.
