# hyprtoggle

A bash cli script for toggling windows in [hyprland]

## Introduction

`hyprtoggle` simplifies managing application focus: it toggles a single app instance by hiding it when it's already focused, and showing or focusing it when it's not.

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

### Required Arguments

- `--class <class>` - Window class to toggle (e.g., `org.wezfurlong.wezterm`). Can be found using `hyprctl clients
- `--exec <command>` - Command to execute if window doesn't exist

### Optional Arguments

- `--on-hide <command>` - Command to execute when hiding window
- `--on-focus <command>` - Command to execute when focusing window
- `--workspace <name>` - Workspace to hide window to (default: `special:hidden`)
- `--help` - Show help message

## Examples

### Basic terminal toggle

```bash
hyprtoggle --class "kitty" --exec "kitty"
```

### With custom workspace

```bash
hyprtoggle --class "firefox" --exec "firefox" --workspace "special:browser"
```

### Hyprland Configuration Examples

```conf
# Toggle browser with hooks (play sounds on open/close)
bind = $mainMod, b, exec, hyprtoggle --class "google-chrome" --exec "google-chrome-stable" --on-hide "ffplay -nodisp -autoexit $HOME/close.mp3" --on-focus "ffplay -nodisp -autoexit $HOME/open.mp3"

# Simple toggle without hooks
bind = $mainMod, grave, exec, hyprtoggle --class "kitty" --exec "kitty"
```

## How It Works

1. **Window exists and is focused**: Hides the window to the specified workspace
2. **Window exists but not focused**: Brings the window to the current workspace and focuses it
3. **Window doesn't exist**: Launches the application using the provided command

The script also handles fullscreen windows by automatically exiting fullscreen mode before toggling.

## Dependencies

- [Hyprland](https://hyprland.org/)
- `jq` - for JSON parsing
- `bash` - version 4.0 or higher

## License

MIT
