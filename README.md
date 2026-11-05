# WispSt4ble

Hosting + release repository for the **WispSt4ble** Minecraft server (Paper 1.21.11):
custom plugins, resource packs, and licensing.

> **Proprietary — All Rights Reserved.** This code is published so it can be
> inspected and so clients can download resource packs. It is **not** open source.
> See [`LICENSE`](LICENSE) before doing anything with it.

---

## What's here

| Folder / file | Contents |
|---|---|
| [`protected/`](protected/) | Newest build of each plugin, **obfuscated**, plus `WispGuard`. These are the jars you run. |
| [`plugins/`](plugins/) | Historical / plain plugin builds (version history). |
| [`licensing/`](licensing/) | `wisp-keygen.jar` and the [licensing guide](licensing/LICENSING.md). |
| `*.zip` (root) | Resource packs served to clients (ItemRenamer pack, SculkSMP, WaterfallReality, tooltips). |
| [`LICENSE`](LICENSE) | Proprietary all-rights-reserved license. |

The **source code** lives in a separate private repository, not here.

## Plugins (current versions)

| Plugin | Version | Role |
|---|---|---|
| PlayerKits3 | V68 | Kits, vaults, auction house, crates, editor |
| WispUtils | V53 | PvP utilities, scoreboard, arenas, optimization |
| WispXAnticheat | V30 | Anticheat + staff tools |
| PVPBOTSX | V17 | Practice bots |
| ThePort | V17 | Division rod / teleport items |
| WardenBoss | V9 | Custom Warden boss fight |
| ItemRenamer | V37 | Resource-pack item models / renamer |
| WispGuard | 1.0.0 | License gate for the above (offline, non-breaking) |

Verify a download against [`protected/SHA256SUMS.txt`](protected/SHA256SUMS.txt):

```
sha256sum -c SHA256SUMS.txt
```

## Protection

These builds carry two layers, both designed never to break a running server:

1. **Obfuscation.** Internal method and field names are stripped to single
   letters and all debug/line information is removed. Public API, class names,
   `plugin.yml`, and resource packs are left intact so reflection and the game
   client keep working.
2. **License keys.** `WispGuard` checks a signed `license.key` locally (no
   network). It defaults to *warn* mode and only ever disables the specific
   listed plugins in *enforce* mode — never the whole server. See the
   [licensing guide](licensing/LICENSING.md).

## Resource pack hosting

The resource-pack zips in the repo root are downloaded directly by game clients.
`ItemRenamer-pack.zip` must stay byte-identical to the copy bundled inside the
installed ItemRenamer jar (the server hashes the hosted copy). Raw URL pattern:

```
https://github.com/mattron1c/Resourcepackhost/raw/main/<file>
```

## Releases

Tagged releases bundle the obfuscated jars for each server update. See the
[Releases](../../releases) page.
