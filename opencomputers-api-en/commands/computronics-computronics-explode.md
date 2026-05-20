# explode

## Summary

Arm a self-destruct card or server self-destructor with optional confirmation and countdown display modes.

## Availability

- Repository: `computronics`
- System: `computronics`
- Type: `command`

## Source

- `Computronics/src\main\resources\assets\computronics\loot\explode\usr\bin\explode.lua`

## Syntax

```lua
explode [-s|-t] [-y] TIME
```

## Purpose

Validate the requested fuse time, optionally ask the user to confirm, start the self-destruct timer, and then either remain silent, update a countdown inline inside the current shell row, or switch to a dedicated full-screen countdown view until detonation.

## Parameters

- `TIME`: Fuse length in seconds. It must be greater than `0` and not larger than `100000`.

## Options

- `-s`: Start the fuse silently without drawing any countdown.
- `-t`: Keep the shell visible and update the countdown on the current line.
- `-y`: Skip the confirmation prompt.

## Examples

```lua
explode 10
```

Ask for confirmation, then start a 10-second fuse and switch to the full-screen countdown view.

```lua
explode -t 30
```

Start a 30-second fuse and render the countdown inline in the current shell.

```lua
explode -s -y 5
```

Immediately arm a 5-second fuse without confirmation and without any countdown display.

## Notes

- `-s` and `-t` are mutually exclusive; the script rejects the combination before arming the fuse.
- The command requires either a `self_destruct` component or a `server_destruct` component.
- In non-silent modes, the fuse is started before GPU and screen availability is checked. If no terminal is available, arming still succeeds and only the countdown display fails.
