# hyprtoggle

A simple CLI-based toggle script for Hyprland windows.

## Introduction

`hyprtoggle` helps overcome Hyprland's default behavior of opening new instances of applications. Instead of spawning duplicate windows, this script toggles a single instance: hiding it when focused, and showing/focusing it when not visible.

Think of it as similar to [hyprshell](https://github.com/H3rmt/hyprshell), but purely command-line based without a GUI or daemon. Just a simple bash script you can bind directly to keybindings in your Hyprland configuration.

## Installation

1. Clone this repository:
```bash
git clone https://github.com/yourusername/hyprtoggle.git
cd hyprtoggle
```

2. Make the script executable:
```bash
chmod +x hyprtoggle
```

3. (Optional) Add to your PATH:
```bash
sudo ln -s "$(pwd)/hyprtoggle" /usr/local/bin/hyprtoggle
```

## Usage

```bash
hyprtoggle --class <window_class> --exec <command> [OPTIONS]
```

### Required Arguments

- `--class <class>` - Window class to toggle (e.g., `org.wezfurlong.wezterm`)
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

### Hyprland Configuration

Add bindings to your `hyprland.conf`:

```conf
# Toggle dropdown terminal with sound effects
bind = $mainMod, t, exec, hyprtoggle --class "org.wezfurlong.wezterm" --exec "wezterm" --on-hide "ffplay -nodisp -autoexit $HOME/.files/sounds/windwaker/pause-close.mp3" --on-focus "ffplay -nodisp -autoexit $HOME/.files/sounds/windwaker/pause.mp3"

# Toggle browser with sound effects
bind = $mainMod, b, exec, hyprtoggle --class "google-chrome" --exec "google-chrome-stable" --on-hide "ffplay -nodisp -autoexit $HOME/.files/sounds/kingdom-hearts/pause-close.mp3" --on-focus "ffplay -nodisp -autoexit $HOME/.files/sounds/kingdom-hearts/pause.mp3"

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
