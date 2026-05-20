# shutdown

## Summary

Immediately clear the terminal and power off the Plan9k machine.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\shutdown.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\shutdown`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\shutdown`

## Syntax

```lua
shutdown
```

## Purpose

Clear the current terminal and immediately invoke `computer.shutdown()`.

## Examples

```lua
shutdown
```
Power off the Plan9k machine immediately.
