# shutdown

## Summary

Immediately clear the terminal and power off the computer.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\shutdown.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\shutdown`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\shutdown`

## Syntax

```lua
shutdown
```

## Purpose

Clear the current terminal through `tty.clear()` and then call `computer.shutdown()` with no delay.

## Examples

```lua
shutdown
```
Power off the computer immediately.
