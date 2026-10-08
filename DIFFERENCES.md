# DIFFERENCES — my-frame-autopass vs. upstream frame-autopass

This repository is a **fork** of **[bod09/frame-autopass](https://github.com/bod09/frame-autopass)**
(MIT © 2026 bod09). It is **not a rewrite**: the passthrough-switching logic is upstream's, kept
byte-for-byte, so that fixes and SteamVR-compatibility updates can be pulled in cleanly.

- **Fork base:** upstream `v0.2.0` — commit `1ba313b` ("Settings panel in the app launcher, with
  automatic, always colour and always night vision modes"). Tagged here as `upstream-v0.2.0`.
- **What this fork is:** a **host-side convenience layer** and documentation. **No changes to the
  compiled C++ programs** (`autopassd`, `autopass`, `autopass-settings`) — they are identical to upstream.

## Added by this fork

| Path | What | Why |
| --- | --- | --- |
| `wrapper/framepass` | A Bash wrapper you run **on your Mac** that drives the on-device `autopass` CLI over SSH (`status`, `mode`, `update`, `report`, `guard`, `uninstall`) with a pretty status view. | Manage the headset from your computer without logging in, and get a readable status instead of raw JSON. |
| `wrapper/README.md` | Install/usage notes for the wrapper. | |
| `DIFFERENCES.md` | This file. | Record exactly how the fork differs, so it never drifts silently from upstream. |
| `README.md` | Upstream README, with a short "About this fork" banner pointing here. | |

## Deliberately NOT changed

Everything that affects behaviour on the headset stays upstream:

- `src/**` — the daemon (`autopassd`), CLI, sensors, light policy, private camera interface, shared-memory reader, config.
- `install.sh` — installer/updater (systemd **user** service, checksum verification, `$HOME`-only).
- `tools/`, `tests/`, `Makefile`, `FINDINGS.md`, `LICENSE`.

Consequences of keeping it untouched:

- **Upstream behaviour is preserved**, including the "Check for updates" action (it goes online only
  when you press it). There is no telemetry and nothing else touches the network.
- The fragile part — the private `IVRCameraPassthroughInternal_001` vtable slots — is upstream's, and
  upstream's guard (fingerprint before switching, stop safely on mismatch) is intact.

## Keeping in sync with upstream

The base commit is recorded on the `upstream-v0.2.0` tag; `upstream` is a configured remote.

```sh
git fetch upstream --tags
git log --oneline main..upstream/main         # what upstream has that we don't
git diff upstream/main -- src install.sh      # should be empty: our C++ == upstream's
git merge upstream/main                        # or: git rebase upstream/main
```

If `git diff upstream/main -- src install.sh` is ever **not** empty, the fork has diverged in the
compiled logic — either bring it back in line or update this file to explain why.

## License / attribution

MIT, © 2026 bod09 (upstream). Modifications in this fork are likewise MIT. See `LICENSE`.
