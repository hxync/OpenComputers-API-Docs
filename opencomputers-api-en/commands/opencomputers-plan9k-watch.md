# watch

## Summary

Repeatedly clear the terminal and rerun a command once per second.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\watch.lua`

## Syntax

```lua
watch COMMAND [ARGS...]
```

## Purpose

Clear the screen, spawn the requested command using the provided argument list, wait for that child process to finish, sleep one second, and then repeat forever.

## Parameters

- `COMMAND`: Program path or executable name to run repeatedly.
- `ARGS...`: Optional arguments passed to the command every time it is spawned.

## Examples

```lua
watch ps
```
Refresh the process list once per second.

```lua
watch ls /mnt
```
Continuously clear the terminal and show the contents of `/mnt`.

## Notes

- The refresh interval is hard-coded to one second.
