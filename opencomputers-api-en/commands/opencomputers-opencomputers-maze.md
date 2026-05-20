# maze

## Summary

Generate and dig a random maze with a robot using depth-first search.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\maze\usr\bin\maze.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\maze\usr\man\maze.man`

## Syntax

```lua
maze WIDTH LENGTH
```
```lua
maze WIDTH LENGTH HEIGHT
```
```lua
maze WIDTH LENGTH HEIGHT DIAMETER
```
```lua
maze WIDTH LENGTH HEIGHT DIAMETER REFUEL
```

## Purpose

Build an in-memory visitation grid, explore it with a depth-first backtracking algorithm, and physically carve the maze in the world. Width and length must both be at least `2`; height defaults to `1`, cell diameter defaults to `1`, and refueling defaults to `false`.

## Parameters

- `WIDTH`: Maze width in cells.
- `LENGTH`: Maze length in cells.
- `HEIGHT`: Vertical height of each maze cell in blocks.
- `DIAMETER`: Horizontal size of each cell in blocks.
- `REFUEL`: Literal `true` or `false` indicating whether the generator upgrade should auto-refuel.

## Examples

```lua
maze 10 10
```
Dig a `10 x 10` maze using the default height, diameter, and refuel settings.

```lua
maze 10 10 2 3 true
```
Dig a `10 x 10` maze with cell height `2`, cell diameter `3`, and automatic refueling enabled.

## Notes

- If `REFUEL` is `true`, the robot must have a generator upgrade installed.
- Increasing `DIAMETER` increases the real Minecraft footprint of the maze, not just the logical cell size.
