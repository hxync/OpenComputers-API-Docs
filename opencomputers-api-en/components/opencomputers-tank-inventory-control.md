# opencomputers-tank-inventory-control

## Summary

This page documents callbacks that move fluid between the host's internal tanks and fluid container items held in its inventory.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: tank-capable hosts such as `robot`
- Backing trait: `TankInventoryControl`

## Numbering Rules

- Inventory slots are `1`-based.
- Internal tank indexes are `1`-based.
- Fluid amounts are measured in millibuckets.

## API

### getTankLevelInSlot

- Syntax: `device.getTankLevelInSlot([slot])`
- Returns:
  - Success: `number`
  - Failure: `nil, "item is not a fluid container"`
- Purpose: Returns the current fluid amount inside the selected or specified container item.

### getTankCapacityInSlot

- Syntax: `device.getTankCapacityInSlot([slot])`
- Returns:
  - Success: `number`
  - Failure: `nil, "item is not a fluid container"`
- Purpose: Returns the maximum capacity of the selected or specified container item.

### getFluidInTankInSlot

- Syntax: `device.getFluidInTankInSlot([slot])`
- Returns:
  - Success: fluid description table
  - Failure: `nil, "item is not a fluid container"` or `nil, "not enabled in config"`
- Purpose: Returns a fluid description for the selected or specified container item.

### getFluidInInternalTank

- Syntax: `device.getFluidInInternalTank([tank])`
- Returns:
  - Success: fluid description table or `nil`
  - Failure: `nil, "not enabled in config"`
- Purpose: Returns a fluid description for one internal tank.

### drain

- Syntax: `device.drain([amount])`
- Returns:
  - Success: `true, movedAmount`
  - Failure: `nil, reason`
- Purpose: Moves fluid from the selected inventory slot into the selected internal tank.

Possible failure reasons include:

- `nothing selected`
- `item is empty or not a fluid container`
- `tank is full`
- `incompatible fluid`
- `incompatible or no fluid`
- `no tank`

### fill

- Syntax: `device.fill([amount])`
- Returns:
  - Success: `true, movedAmount`
  - Failure: `nil, reason`
- Purpose: Moves fluid from the selected internal tank into the selected inventory slot.

Possible failure reasons include:

- `nothing selected`
- `item is full or not a fluid container`
- `tank is empty`
- `incompatible or no fluid`
- `no tank`

## Practical Notes

- The callbacks support both vanilla-style fillable containers and items implementing `IFluidContainerItem`.
- When a container is consumed and replaced by another item, the result is inserted back into the inventory if possible; otherwise it is spawned into the world.

## Example

```lua
local robot = require("robot")

robot.select(1)
robot.selectTank(1)

print("Container level:", robot.getTankLevelInSlot())
print("Container capacity:", robot.getTankCapacityInSlot())

local ok, moved = robot.drain(1000)
print("Moved from container into tank:", ok, moved)

local ok2, moved2 = robot.fill(500)
print("Moved from tank into container:", ok2, moved2)
```

## Related

- `opencomputers-tank-control`
- `opencomputers-world-fluid-container-analytics`
- `robot`
