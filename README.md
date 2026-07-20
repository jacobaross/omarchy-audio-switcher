# Omarchy Audio Switcher

A small Bash utility for fast, explicit switching between a headset and
speakers on an Omarchy desktop using PipeWire, WirePlumber, and `wpctl`.

It uses stable PipeWire node names for configuration. Numeric object IDs are
resolved at runtime and are never stored or hard-coded.

## Commands

```text
audio headset
audio speakers
audio toggle
audio status
```

`audio status` prints `Headset`, `Speakers`, or `Unknown`. Successful switches
show a desktop notification when `notify-send` is available. Missing or powered
off devices produce a helpful error and a non-zero exit status.

## Requirements

- Bash
- PipeWire
- WirePlumber 0.5 or later
- `wpctl`
- `notify-send` (optional)

To move existing streams when the default changes, enable WirePlumber's
`linking.follow-default-target` setting.

## Install

```bash
git clone https://github.com/jacobaross/omarchy-audio-switcher.git
cd omarchy-audio-switcher
install -Dm755 audio ~/.local/bin/audio
```

Make sure `~/.local/bin` is in your `PATH`.

## Configure

Find the stable names of your sinks:

```bash
wpctl status --name
```

Install and edit the example configuration:

```bash
install -Dm644 config.example ~/.config/audio-switcher/config
${EDITOR:-nano} ~/.config/audio-switcher/config
```

You can instead set `AUDIO_HEADSET_SINK` and `AUDIO_SPEAKERS_SINK` environment
variables. Environment variables take precedence over the configuration file.
Set `AUDIO_SWITCHER_CONFIG` to use a different configuration path.

## Hyprland bindings

Add these lines to `~/.config/hypr/bindings.conf`:

```ini
bind = SUPER, F10, exec, audio toggle
bind = SUPER, F11, exec, audio headset
bind = SUPER, F12, exec, audio speakers
```

Hyprland normally reloads automatically after the file is saved. You can also
reload and validate it explicitly:

```bash
hyprctl reload
hyprctl configerrors
```

## License

MIT
