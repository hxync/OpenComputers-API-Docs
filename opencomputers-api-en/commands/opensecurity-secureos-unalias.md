# unalias

## Summary

Remove a SecureOS shell alias and print what mapping was deleted.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\unalias.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\unalias`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unalias`

## Syntax

```lua
unalias NAME
```

## Purpose

Look up the requested alias. If it exists, delete it and print the removed `name -> value` mapping; otherwise report `no such alias`.

## Parameters

- `NAME`: Alias name to remove.

## Examples

```lua
unalias ll
```
Remove the alias `ll` and print its previous target.
