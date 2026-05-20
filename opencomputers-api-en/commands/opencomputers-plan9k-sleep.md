# sleep

## Summary

Pause for one or more time intervals.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sleep.lua`

## Syntax

```lua
sleep NUMBER[SUFFIX]...
```

## Purpose

Sleep for the sum of all provided intervals. Each interval may use seconds, minutes, hours, or days, and fractional values are accepted.

## Parameters

- `NUMBER[SUFFIX]...`: One or more non-negative intervals. Supported suffixes are `s`, `m`, `h`, and `d`; omitted suffix means seconds.

## Options

- `--help`: Print the built-in help text.

## Examples

```lua
sleep 5
```
Pause for five seconds.

```lua
sleep 1.5m 2s
```
Pause for one minute and thirty-two seconds in total.

## Notes

- Invalid options and negative durations are rejected with a usage error.
- An `interrupted` signal ends the wait early.
