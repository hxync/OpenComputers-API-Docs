# ps

## Summary

Display the current process tree and basic runtime counters.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\ps.lua`

## Syntax

```lua
ps
```

## Purpose

Inspect the in-memory process table and print one row per process or thread root. The output includes PID, event-handler count, thread count, handle count, and the recorded command name.

## Examples

```lua
ps
```
Print the current process tree.

## Notes

- This implementation reads non-public process internals and is explicitly marked in source as unstable.
- Child commands are indented with a tree marker so parent-child relationships are visible in the `CMD` column.
