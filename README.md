# hyprtoggle

## Introduction

`hyprtoggle` is a bash cli that simplifies focusing and toggling applications in [hyperland](https://hypr.land/).

## Dependencies

| Tool | Purpose |
|------|---------|
| `bash` 4.0+ | Runtime (associative arrays) |
| [`hyprctl`](https://wiki.hyprland.org/Configuring/Using-hyprctl/) | Querying and dispatching Hyprland window commands |
| [`jq`](https://jqlang.org) | Parsing JSON output from `hyprctl` |
| `awk` | Sleep delay arithmetic |
| `fuzzel` *(optional)* | Default picker for `--cycle-on-multiple`; any dmenu-compatible tool works |

## Installation

Install the standalone script to your desired location:

```bash
# customize the path as needed, assumed to be in PATH:
desired_path="$HOME/.local/bin/hyprtoggle" && \
desired_version="main" && \
curl -o "$desired_path" "https://raw.githubusercontent.com/surgiie/hyprtoggle/$desired_version/hyprtoggle" && chmod +x "$desired_path"
```


## Usage

```bash
hyprtoggle --class <window_class> --exec <command> [OPTIONS]
```

### Arguments / Options

| Flag | Description | Default |
|------|-------------|---------|
| `--class <class>` | Window class to toggle (required unless `--exec` is used alone) | — |
| `--exec <command>` | Command to launch the window if none is found | — |
| `--on-hide <command>` | Hook command to run when hiding a window | — |
| `--on-focus <command>` | Hook command to run when focusing a window | — |
| `--on-launch <command>` | Hook command to run when launching a new window | — |
| `--cycle-on-multiple` | When multiple instances match `--class`, prompt to pick one | false |
| `--picker <command>` | Picker command used with `--cycle-on-multiple` | `fuzzel --dmenu` |
| `--workspace <name>` | Workspace to hide the window to | `special:hidden` |
| `--wait-for-exit` | Block until the toggled window closes | false |
| `--version` | Print version and exit | — |
| `--help` | Print help and exit | — |

At least one of `--class` or `--exec` is required.

- **`--class` only** — toggle an already-running window; exit with an error if none is found.
- **`--exec` only** — launch the command and maximize the resulting window (no toggle).
- **`--class` + `--exec`** — toggle if a matching window exists, otherwise launch it.

### Examples

Toggle a WezTerm window, launching it if it isn't open yet:

```bash
hyprtoggle --class org.wezfurlong.wezterm --exec wezterm
```

Toggle Firefox and run a hook each time it is hidden:

```bash
hyprtoggle --class firefox --exec firefox --on-hide "notify-send 'Firefox hidden'"
```

Just open a floating scratchpad (no class tracking, no toggle):

```bash
hyprtoggle --exec "wezterm start -- btop"
```

Hide the window to a named special workspace instead of the default:

```bash
hyprtoggle --class org.wezfurlong.wezterm --exec wezterm --workspace special:scratch
```

When multiple WezTerm windows are open, prompt with `rofi` to pick one:

```bash
hyprtoggle --class org.wezfurlong.wezterm --exec wezterm --cycle-on-multiple --picker "rofi -dmenu"
```

Open a window and block until it exits (useful in scripts):

```bash
hyprtoggle --exec "wezterm start -- htop" --wait-for-exit
```

### Hooks

Hooks are arbitrary shell commands run at specific points in the toggle lifecycle. They execute in a login shell (`bash -l`) so your `~/.bashrc`, `~/.profile`, and `PATH` are available.

| Hook | When it runs |
|------|-------------|
| `--on-launch <command>` | Once, when the window is first opened |
| `--on-focus <command>` | Each time the window is brought forward |
| `--on-hide <command>` | Each time the window is hidden |

A failed hook prints a warning to stderr but does not abort the toggle.

**Send a desktop notification when the window is hidden:**

```bash
hyprtoggle --class firefox --exec firefox \
    --on-hide "notify-send 'Firefox' 'Window hidden'"
```

**Play a sound on launch and focus:**

```bash
hyprtoggle --class org.wezfurlong.wezterm --exec wezterm \
    --on-launch "paplay /usr/share/sounds/freedesktop/stereo/bell.oga" \
    --on-focus "paplay /usr/share/sounds/freedesktop/stereo/message.oga"
```

**Run a script on hide:**

```bash
hyprtoggle --class org.wezfurlong.wezterm --exec wezterm \
    --on-hide "$HOME/.local/bin/my-script.sh"
```
