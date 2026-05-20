# go

## Summary

Move or turn a robot by a specified positive count.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\lua\component\robot\bin\go.lua`

## Syntax

```lua
go DIRECTION [COUNT]
```

## Purpose

Execute one robot movement or turn operation repeatedly. Supported directions are `forward`, `back`, `up`, `down`, `left`, and `right`, and the count defaults to one.

## Parameters

- `DIRECTION`: One of `forward`, `back`, `up`, `down`, `left`, or `right`.
- `COUNT`: Optional positive integer repetition count. Fractional values are floored.

## Examples

```lua
go forward
```
Move the robot one block forward.

```lua
go left 2
```
Turn the robot left twice.

```lua
go up 3
```
Move the robot upward three times.

## Notes

- The command validates the count but does not stop early if a robot action fails; it simply calls the selected robot API repeatedly.
