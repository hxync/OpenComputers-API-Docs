# build

## Summary

Have a robot build a layered structure from a `0`/`1` schematic plan file.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\builder\usr\bin\build.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\builder\usr\man\build.man`

## Syntax

```lua
build PLAN
```
```lua
build PLAN FROM TO
```

## Purpose

Read a plan file where `0` means air, `1` means a solid block, and lines starting with `#` separate vertical layers. The robot analyzes the model, estimates required materials, optionally resumes from a saved `.state` file, and then builds the selected slab range while returning to its chest for blocks and charging when needed.

## Parameters

- `PLAN`: Plan file to load. The resolver appends `.plan` when no suffix is supplied.
- `FROM`: Zero-based starting slab index along the plan depth axis.
- `TO`: Number of slabs to build starting at `FROM`. If omitted, build the remaining slabs.

## Examples

```lua
build pyramid.plan
```
Analyze `pyramid.plan`, ask for confirmation, and build the full structure.

```lua
build some.plan 0 2
```
Build the slab range starting at `0` and spanning `2` depth slices from `some.plan`.

```lua
build used.plan
```
Resume a previously interrupted build when a matching `.state` file is present.

## Notes

- The robot expects a chest in front of it, building material in that chest, and one stack of dirt in inventory slot `1`.
- Regular building blocks are pulled into slots `2` through `16`.
- If energy falls dangerously low away from the chest, the script saves state and shuts the robot down.
