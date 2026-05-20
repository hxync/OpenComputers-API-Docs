# opencomputers-tank-control

## Summary

This page documents internal tank selection and transfer callbacks. These APIs operate on tanks built into the host device itself.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: exposed by tank-capable hosts such as `robot`
- Backing trait: `TankControl`

## Tank Numbering

Lua-facing tank indexes are `1`-based.

## API

### tankCount

- Syntax: `device.tankCount()`
- Returns: `number`
- Purpose: Returns how many internal tanks the host has.

### selectTank

- Syntax: `device.selectTank([index])`
- Returns: `number`
- Purpose: Reads or changes the currently selected internal tank.

### tankLevel

- Syntax: `device.tankLevel([index])`
- Returns: `number`
- Purpose: Returns the current fluid amount in the specified or selected tank.

Empty tanks return `0`.

### tankSpace

- Syntax: `device.tankSpace([index])`
- Returns: `number`
- Purpose: Returns the remaining free capacity of the specified or selected tank.

### compareFluidTo

- Syntax: `device.compareFluidTo(index)`
- Returns: `boolean`
- Purpose: Compares the selected tank with another internal tank by fluid type.

Behavior:

- Two empty tanks compare as `true`.
- One empty and one filled tank compare as `false`.

### transferFluidTo

- Syntax: `device.transferFluidTo(index[, count])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Moves fluid from the selected tank into another internal tank.

Possible failure reasons:

- `incompatible or no fluid`
- `invalid index`

Behavior details:

- `count` defaults to `1000` millibuckets.
- If source and destination are the same tank, the call returns `true`.
- If direct transfer is impossible but both tanks can hold each other's current contents, the implementation swaps the fluids.

## Example

```lua
local robot = require("robot")

print("Internal tank count:", robot.tankCount())
print("Selected tank:", robot.selectTank())

robot.selectTank(1)
print("Selected tank level:", robot.tankLevel())
print("Selected tank free space:", robot.tankSpace())
print("Same fluid as tank 2:", robot.compareFluidTo(2))

local ok, reason = robot.transferFluidTo(2, 500)
print("Transfer result:", ok, reason)
```

## Related

- `opencomputers-tank-inventory-control`
- `opencomputers-tank-world-control`
- `robot`
