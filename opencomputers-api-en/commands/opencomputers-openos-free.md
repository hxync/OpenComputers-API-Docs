# free

## Summary

Print total, used, and free Lua memory.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\free.lua`

## Syntax

```lua
free
```

## Purpose

Sample `computer.freeMemory()` repeatedly while yielding to the scheduler so garbage collection can run, then print `Total`, `Used`, and `Free` memory values based on the highest free-memory sample observed.

## Examples

```lua
free
```
Print the current Lua memory totals.

## Notes

- The `Used` and `Free` values are an estimate after repeated garbage-collection opportunities, not a strict instantaneous snapshot.
