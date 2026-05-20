# opencomputers-world-control

## Summary

This page documents the simple adjacent-block detection callback used by agents such as robots.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: usually part of `robot` or other agent-like devices
- Backing trait: `WorldControl`

## API

### detect

- Syntax: `device.detect(side)`
- Returns: `boolean, string`
- Purpose: Checks what occupies the adjacent block space on the specified side.

Return value meanings:

- First value: whether the side is considered occupied or blocked for interaction purposes.
- Second value: a category string describing what was found.

Known category strings in this source version:

- `air`
- `entity`
- `liquid`
- `replaceable`
- `passable`
- `solid`

Interpretation notes:

- `air` returns `false, "air"`.
- `entity`, `passable`, and `solid` return `true` with their respective labels.
- `liquid` and `replaceable` depend on the result of a break-permission style event check.

## Example

```lua
local robot = require("robot")
local sides = require("sides")

local blocked, kind = robot.detect(sides.front)
print("Blocked:", blocked)
print("Kind:", kind)
```

## Related

- `opencomputers-inventory-world-control`
- `robot`
