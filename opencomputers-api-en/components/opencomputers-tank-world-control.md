# opencomputers-tank-world-control

## Summary

This page documents callbacks that move fluid between the host's selected internal tank and adjacent world tanks.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: tank-capable hosts such as `robot`
- Backing trait: `TankWorldControl`

## API

### compareFluid

- Syntax: `device.compareFluid(side[, tank])`
- Returns: `boolean`
- Purpose: Compares the selected internal tank with a tank on the specified adjacent side.

Behavior:

- If `tank` is omitted, the call checks whether any tank on that side contains the same fluid type.
- If `tank` is provided, it targets one specific adjacent tank index.

### drain

- Syntax: `device.drain(side[, amount])`
- Returns:
  - Success: `true, movedAmount`
  - Failure: `nil, reason`
- Purpose: Pulls fluid from an adjacent tank into the selected internal tank.

Possible failure reasons:

- `tank is full`
- `incompatible or no fluid`
- `no tank selected`

### fill

- Syntax: `device.fill(side[, amount])`
- Returns:
  - Success: `true, movedAmount`
  - Failure: `nil, reason`
- Purpose: Pushes fluid from the selected internal tank into an adjacent tank.

Possible failure reasons:

- `tank is empty`
- `incompatible or no fluid`
- `no space`
- `no tank selected`

## Practical Notes

- `amount` defaults to `1000` millibuckets.
- Adjacent tank indexes are `1`-based when specified.
- Passing an amount of `0` is treated as a valid no-op style request by the underlying implementation.

## Example

```lua
local robot = require("robot")
local sides = require("sides")

robot.selectTank(1)

print("Same fluid as left tank:", robot.compareFluid(sides.left, 1))

local ok, moved = robot.drain(sides.left, 1000)
print("Pulled from world tank:", ok, moved)

local ok2, moved2 = robot.fill(sides.right, 500)
print("Pushed into world tank:", ok2, moved2)
```

## Related

- `opencomputers-tank-control`
- `opencomputers-world-tank-analytics`
- `robot`
