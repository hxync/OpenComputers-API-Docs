# dig

## Summary

Make a robot excavate a square shaft until it hits an unbreakable block.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\dig\usr\bin\dig.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\dig\usr\man\dig.man`

## Syntax

```lua
dig SIZE
```

## Purpose

Dig a square area of the requested size in a layered zig-zag pattern. When the robot inventory fills up, it returns to the starting position, turns toward the side that was originally behind it, and drops everything before resuming.

## Parameters

- `SIZE`: Side length of the square area to excavate.

## Options

- `-s`: Shut the robot down after excavation finishes.

## Examples

```lua
dig 3
```
Excavate a `3 x 3` shaft downward until the robot can no longer continue.

## Notes

- The implementation performs no proactive energy management.
- Drops are emitted toward the side that was behind the robot when the command started, so placing a chest there allows automatic collection.
