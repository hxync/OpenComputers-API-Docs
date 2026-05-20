# useradd

## Summary

Add a player to the computer's authorized user list.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\useradd.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\useradd`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\useradd`

## Syntax

```lua
useradd NAME
```

## Purpose

Add the specified logged-in player to the machine's authorized user list. Once at least one user is registered, only listed users may interact with the computer.

## Parameters

- `NAME`: Exact player name to authorize.

## Examples

```lua
useradd Steve
```
Authorize the logged-in player `Steve` to use the computer.

## Notes

- Player names are case sensitive.
- Users can later be removed again with `userdel`.
- Computer ownership can be disabled in the configuration.
