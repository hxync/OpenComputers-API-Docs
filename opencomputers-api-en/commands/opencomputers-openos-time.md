# time

## Summary

Measure wall-clock and CPU time for a shell command.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\time.lua`

## Syntax

```lua
time [COMMAND [ARGS...]]
```

## Purpose

Record `computer.uptime()` and `os.clock()` before and after executing a shell command, then print `real` and `cpu` duration lines in minutes and seconds.

## Parameters

- `COMMAND`: Optional shell command to execute and time.
- `ARGS...`: Optional arguments forwarded to the timed command.

## Examples

```lua
time ls /bin
```
Run `ls /bin`, then print real and CPU timing lines.

```lua
time
```
Print near-zero timing values without executing another command.

## Notes

- The command returns the last exit code of the timed shell command.
