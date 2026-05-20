# opencomputers-world-fluid-container-analytics

## Summary

This page documents read-only inspection callbacks for fluid container items stored in adjacent inventories.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: usually part of `transposer` or `inventory_controller` style world-inspection devices
- Backing trait: `WorldFluidContainerAnalytics`

## General Rules

- Side numbers target adjacent inventories.
- Inventory slots are `1`-based.
- These callbacks require item-stack inspection to be enabled in the OpenComputers config.

## API

### getContainerCapacityInSlot

- Syntax: `device.getContainerCapacityInSlot(side, slot)`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Returns the total capacity of the fluid container item in the specified adjacent inventory slot.

### getContainerLevelInSlot

- Syntax: `device.getContainerLevelInSlot(side, slot)`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Returns the current fluid amount in the specified adjacent inventory slot.

### getFluidInContainerInSlot

- Syntax: `device.getFluidInContainerInSlot(side, slot)`
- Returns:
  - Success: fluid description table or `nil`
  - Failure: `nil, reason`
- Purpose: Returns a fluid description for the specified adjacent inventory slot.

Common failure reasons:

- `not enabled in config`
- `no inventory`
- `item is not a fluid container`

## Example

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local cap, reason = transposer.getContainerCapacityInSlot(sides.front, 1)
print("Container capacity:", cap, reason)

local level = transposer.getContainerLevelInSlot(sides.front, 1)
print("Container level:", level)

local fluid = transposer.getFluidInContainerInSlot(sides.front, 1)
if fluid then
  print("Fluid name:", fluid.name)
end
```

## Related

- `opencomputers-fluid-container-transfer`
- `opencomputers-world-inventory-analytics`
