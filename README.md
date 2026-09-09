# Baritone (Evil)

<p align="center">
  <a href="https://github.com/evilinc-labs/baritone-evil/releases/"><img src="https://img.shields.io/github/downloads/evilinc-labs/baritone-evil/total.svg" alt="GitHub All Releases"/></a>
  <a href="https://github.com/evilinc-labs/baritone-evil/releases/latest"><img src="https://img.shields.io/github/release/evilinc-labs/baritone-evil.svg" alt="Release"/></a>
  <a href="#baritone-evil"><img src="https://img.shields.io/badge/jars-unobfuscated-brightgreen.svg" alt="Unobfuscated"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-LGPL--3.0%20with%20anime%20exception-green.svg" alt="License"/></a>
</p>

<p align="center">
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-26.2-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-26.1-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-1.21.11-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-1.21.8-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-1.21.5-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/MC-1.21.4-brightgreen.svg" alt="Minecraft"/></a>
  <a href="#downloads"><img src="https://img.shields.io/badge/loader-Fabric-brightgreen.svg" alt="Fabric"/></a>
</p>

<p align="center">
  <a href="https://github.com/evilinc-labs/baritone-evil/issues/"><img src="https://img.shields.io/github/issues/evilinc-labs/baritone-evil.svg" alt="Issues"/></a>
  <a href="https://github.com/evilinc-labs/baritone-evil/issues?q=is%3Aissue+is%3Aclosed"><img src="https://img.shields.io/github/issues-closed/evilinc-labs/baritone-evil.svg" alt="GitHub issues-closed"/></a>
  <a href="https://github.com/evilinc-labs/baritone-evil/pulls/"><img src="https://img.shields.io/github/issues-pr/evilinc-labs/baritone-evil.svg" alt="Pull Requests"/></a>
  <a href="https://github.com/evilinc-labs/baritone-evil/commit/"><img src="https://img.shields.io/github/commits-since/evilinc-labs/baritone-evil/v1.13.1-evil.svg" alt="Commits since first release"/></a>
  <img src="https://img.shields.io/github/languages/code-size/evilinc-labs/baritone-evil.svg" alt="Code size"/>
  <img src="https://img.shields.io/github/repo-size/evilinc-labs/baritone-evil.svg" alt="GitHub repo size"/>
</p>

<p align="center">
  <a href="https://impactclient.net/"><img src="https://img.shields.io/badge/Impact%20integration-v1.2.14%20/%20v1.3.8%20/%20v1.4.6%20/%20v1.5.3%20/%20v1.6.3-brightgreen.svg" alt="Impact integration"/></a>
  <a href="https://github.com/lambda-client/lambda"><img src="https://img.shields.io/badge/Lambda%20integration-v1.2.17-brightgreen.svg" alt="Lambda integration"/></a>
  <a href="https://github.com/fr1kin/ForgeHax/"><img src="https://img.shields.io/badge/ForgeHax%20%22integration%22-scuffed-yellow.svg" alt="ForgeHax integration"/></a>
  <a href="https://aristois.net/"><img src="https://img.shields.io/badge/Aristois%20add--on%20integration-v1.6.3-green.svg" alt="Aristois add-on integration"/></a>
  <a href="https://rootnet.dev/"><img src="https://img.shields.io/badge/rootNET%20integration-v1.2.14-green.svg" alt="rootNET integration"/></a>
  <a href="https://futureclient.net/"><img src="https://img.shields.io/badge/Future%20integration-v1.2.12%20%2F%20v1.3.6%20%2F%20v1.4.4-red" alt="Future integration"/></a>
  <a href="https://rusherhack.org/"><img src="https://img.shields.io/badge/RusherHack%20integration-v1.2.14-green" alt="RusherHack integration"/></a>
</p>

A Minecraft pathfinder bot, shipped unobfuscated so you can read what you run.

This is a minimal fork of [cabaletta/baritone](https://github.com/cabaletta/baritone). All of the pathfinding, all of the code, and all of the credit are upstream's. What this fork changes is how the jar is packaged.

## Why this fork exists

Upstream publishes three jar variants per release. Two of them, `api` and `standalone`, run their internals through ProGuard, so most class and method names in the jar you actually install are single letters. Upstream's jars are safe, but you cannot casually read them, and both of those variants declare the same Fabric mod id `baritone`, so if two end up in your `mods/` folder the loader silently picks one.

This fork does three things about that:

- **Ships the unobfuscated build.** Class names stay as the authors wrote them, so you can unzip the jar and read every class before you trust it with your account.
- **Renames the mod id** from `baritone` to `baritone-evil`, so it cannot silently collide with an upstream jar in the mod registry.
- **Declares `breaks: baritone`,** so if you do have an upstream jar in there too, Fabric stops with a clear message naming the problem instead of behaving strangely later.

Nothing else is touched. Every release branch is cut directly from an upstream release commit and changes exactly three files: `gradle.properties`, `fabric/build.gradle`, and `fabric/src/main/resources/fabric.mod.json`. There are zero Java source changes, no added dependencies, and no added Maven repositories.

## Verifying that for yourself

You do not have to take any of that on faith. Every release names the exact upstream commit it was built from, and the compiled classes in our jar are byte-for-byte identical to upstream's own `baritone-unoptimized-fabric-*.jar` for the same release:

```sh
# grab both jars, then compare the compiled code
unzip -q baritone-standalone-fabric-1.19.0-evil.jar -d evil
unzip -q baritone-unoptimized-fabric-1.19.0.jar     -d upstream
diff -r evil/baritone upstream/baritone && echo "identical"
```

The only differences anywhere in the archive are the four metadata fields in `fabric.mod.json` listed above. Note that a rebuild will not reproduce the published jar's SHA-256, because this fork skips the upstream packaging step that zeroes zip timestamps. Compare the class files, not the archive hash.

## Downloads

Fabric only. Drop the jar into your `mods/` folder.

| Minecraft | Release | Jar |
|---|---|---|
| 26.2 | [v1.19.0-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.19.0-evil) | [baritone-standalone-fabric-1.19.0-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.19.0-evil/baritone-standalone-fabric-1.19.0-evil.jar) |
| 26.1.x | [v1.18.0-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.18.0-evil) | [baritone-standalone-fabric-1.18.0-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.18.0-evil/baritone-standalone-fabric-1.18.0-evil.jar) |
| 1.21.11 | [v1.17.0-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.17.0-evil) | [baritone-standalone-fabric-1.17.0-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.17.0-evil/baritone-standalone-fabric-1.17.0-evil.jar) |
| 1.21.6, 1.21.7, 1.21.8 | [v1.15.0-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.15.0-evil) | [baritone-standalone-fabric-1.15.0-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.15.0-evil/baritone-standalone-fabric-1.15.0-evil.jar) |
| 1.21.5 | [v1.14.0-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.14.0-evil) | [baritone-standalone-fabric-1.14.0-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.14.0-evil/baritone-standalone-fabric-1.14.0-evil.jar) |
| 1.21.4 | [v1.13.1-evil](https://github.com/evilinc-labs/baritone-evil/releases/tag/v1.13.1-evil) | [baritone-standalone-fabric-1.13.1-evil.jar](https://github.com/evilinc-labs/baritone-evil/releases/download/v1.13.1-evil/baritone-standalone-fabric-1.13.1-evil.jar) |

If an upstream `baritone-*-fabric-*.jar` is already in your `mods/` folder, remove it. Fabric will tell you so on the next launch.

Need Forge, NeoForge, or an older Minecraft version? Those come from [upstream releases](https://github.com/cabaletta/baritone/releases) and are obfuscated.

## How to immediately get started

Type `#goto 1000 500` in chat to go to x=1000 z=500. Type `#mine diamond_ore` to mine diamond ore. Type `#stop` to stop. Also try `#elytra` for Elytra flying in the Nether using fireworks.

For more, read [the usage page](USAGE.md). For other versions or for development, see [Installation & setup](SETUP.md).

## Getting Started

- [Features](FEATURES.md)
- [Installation & setup](SETUP.md)
- [API Javadocs](https://baritone.leijurv.com/)
- [Settings](https://baritone.leijurv.com/baritone/api/Settings.html#field.detail)
- [Usage (chat control)](USAGE.md)

## API

The API is heavily documented, you can find the Javadocs for the latest upstream release [here](https://baritone.leijurv.com/).
Because this fork ships the unobfuscated build, the whole `baritone.api` package is present and reflectable, and the internals are readable too. Class names are unchanged from upstream, so anything written against `baritone.api.*` works without modification.

```java
BaritoneAPI.getSettings().allowSprint.value = true;
BaritoneAPI.getSettings().primaryTimeoutMS.value = 2000L;
BaritoneAPI.getProvider().getPrimaryBaritone().getCustomGoalProcess().setGoalAndPath(new GoalXZ(10000, 20000));
```

## Support and credit

Baritone is written and maintained by [leijurv](https://github.com/leijurv/), Brady, and the upstream contributors. Questions about how Baritone itself works belong in the [Baritone Discord Server](http://discord.gg/s6fRBAUpmr) or on the [upstream issue tracker](https://github.com/cabaletta/baritone/issues) — please do not send them packaging problems that we created.

Open an [issue here](https://github.com/evilinc-labs/baritone-evil/issues) only for things specific to this fork: the mod id, the `breaks` declaration, a missing version, or a bad release artifact.

## License

LGPL-3.0, inherited from upstream Baritone. The source is available at all times, which is the entire point.
