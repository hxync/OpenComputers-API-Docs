# unalias

## Summary

Remove one or more shell aliases.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\unalias.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unalias`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\unalias`

## Syntax

```lua
unalias NAME...
```

## Purpose

Look up each provided alias name and remove it with `shell.setAlias(name, nil)`. Missing aliases are reported individually while the command continues processing later operands.

## Parameters

- `NAME...`: One or more alias names to remove.

## Examples

```lua
unalias ll
```
Remove the alias named `ll`.

```lua
unalias ll la lgrep
```
Remove several aliases in one invocation.

## Notes

- If any alias is missing, the command exits with status `1` after printing `unalias: <name>: not found` for that operand.
