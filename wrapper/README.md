# framepass — control frame-autopass from your Mac

A small Bash wrapper that runs **on your Mac** and drives the `autopass` CLI **on the Steam Frame**
over SSH. Part of the [my-frame-autopass](../DIFFERENCES.md) fork; it changes nothing on the headset.

## Install

```sh
install -m 0755 framepass ~/.local/bin/framepass
# make sure ~/.local/bin is on your PATH
```

## Usage

```sh
framepass status                 # daemon state, pretty-printed
framepass mode auto              # or: colour | ir
framepass update                 # update frame-autopass on the headset
framepass report                 # write a bug-report file on the headset
framepass guard reset            # clear the crash-guard pause
framepass uninstall --purge      # remove frame-autopass from the headset
framepass ssh                    # interactive shell on the Frame
framepass where                  # which host it talks to
```

## Which host it uses

In order: `$FRAME_SSH` if set, then `HOSTNAME` then `LAST_IP_ADDRESS` from `~/.steam-frame.env`,
then `frame`. Override any time:

```sh
FRAME_SSH=steamos@192.168.1.180 framepass status
```

It prefers an SSH host that answers in ≤4 s (so it falls back to the LAN IP when mDNS is flaky).

## Requirements

- `ssh` with key access to the Frame (Developer Mode on). See the vault note *Steam Frame — Auto Passthrough*.
- The headset's own `autopass` installed at `~/.local/share/frame-autopass/` (done by `install.sh`).
