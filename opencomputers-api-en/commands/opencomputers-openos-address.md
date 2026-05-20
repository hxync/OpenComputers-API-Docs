# address

## Summary

Print the component address of the current computer.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\address.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\address`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\address`

## Syntax

```lua
address
```

## Purpose

Write the machine's own component address to standard output. This is useful when you need the address for networking or remote identification but do not have an analyzer item handy.

## Examples

```lua
address
```
Display the address of the computer running the command.
