# kill

## Summary

Send the `kill` signal to a Plan9k process by PID.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\kill.lua`

## Syntax

```lua
kill PID
```

## Purpose

Convert the single argument to a numeric PID and call `os.kill(pid, "kill")`. If the PID is invalid or the signal cannot be delivered, the command raises an error.

## Parameters

- `PID`: Numeric process identifier to terminate.

## Examples

```lua
kill 42
```
Request termination of process `42`.

## Notes

- This implementation always sends the literal `kill` signal and does not support alternate signal names.
