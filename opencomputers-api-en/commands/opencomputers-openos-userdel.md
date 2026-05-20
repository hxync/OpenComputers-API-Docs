# userdel

## Summary

Remove a player from the computer's authorized user list.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\userdel.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\userdel`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\userdel`

## Syntax

```lua
userdel NAME
```

## Purpose

Remove the specified player from the machine's authorized user list.

## Parameters

- `NAME`: Exact player name to remove from authorization.

## Examples

```lua
userdel Steve
```
Remove `Steve` from the authorized user list.

## Notes

- See `useradd` to grant access again later.
