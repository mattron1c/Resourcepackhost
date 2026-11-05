# WispSt4ble Licensing

This explains the license-key system for the WispSt4ble plugins. It is designed
around two rules:

1. **It is fully offline.** A license key is a signed file. The plugins verify it
   locally with an embedded public key — there is **no server call, ever**. Being
   offline can never block, slow, or break anything.
2. **It never bricks your server.** By default enforcement is set to `warn`:
   plugins log their license status and otherwise run exactly as normal. You only
   enable hard enforcement if you want it.

## Pieces

| File | Where | What it is |
|------|-------|-----------|
| `wisp-keygen.jar` | this folder | Tool that creates the signing keys and issues license keys. Runs on your PC, not the server. |
| `WispGuard-1.0.0.jar` | `../protected/` | A tiny plugin that reads one license key and gates the others. |
| `wispguard-private.key` | **your PC only** | The secret signing key. **Never upload this.** |
| `license.key` | `plugins/WispGuard/` on the server | The signed license you issue to yourself. |

## One-time setup

On your PC (needs Java 17+), in an empty folder:

```
java -jar wisp-keygen.jar genkeys
```

This writes `wispguard-private.key` (keep it secret) and prints a **public key**.
The public key is already embedded in the build you were given, so for the shipped
jars you don't need to change anything. If you ever rebuild the plugins yourself,
paste the printed public key into `LicenseVerifier.PUBLIC_KEY_B64`.

## Issuing your license key

```
java -jar wisp-keygen.jar sign --owner "Matt"
```

That writes `license.key`. Options:

```
--owner   "Matt"              who it's for (required)
--plugins *                   or a comma list, e.g. PlayerKits3,WispUtils
--server  *                   free-form label for your records
--expires never               or a date, e.g. 2027-01-01
--note    "whatever you like"
--out     license.key         output path
```

Drop the resulting `license.key` into `plugins/WispGuard/license.key` on your
server and restart (or run `/wispguard reload`).

## WispGuard modes (`plugins/WispGuard/config.yml`)

```yaml
mode: warn      # warn (default) = log only, never disable anything
                # enforce        = disable the protected-plugins if unlicensed
protected-plugins:
  - PlayerKits3
  - WispUtils
  - WispXAnticheat
  - PVPBOTSX
  - ThePort
  - WardenBoss
  - ItemRenamer
```

In `enforce` mode, if no valid license is found WispGuard disables **only** the
plugins you list — never the whole server. Check status in-game with `/wispguard`.

## Keeping the signing key safe

- The **private** key (`wispguard-private.key`) is the only secret. Anyone who has
  it can issue licenses. Keep it on your PC, back it up somewhere private, and
  never commit or upload it. `.gitignore` already blocks `*private*.key`.
- The **public** key is safe to publish — it can only *verify*, not issue.
- If the private key ever leaks, run `genkeys --force` to make a new pair, rebuild
  with the new public key, and reissue your `license.key`.
