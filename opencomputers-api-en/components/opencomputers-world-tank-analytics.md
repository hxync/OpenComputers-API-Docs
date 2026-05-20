# opencomputers-world-tank-analytics

## Summary

This page documents read-only inspection callbacks for tanks and fluid handlers located on adjacent sides.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: commonly exposed by `transposer` and other tank-aware devices
- Backing trait: `WorldTankAnalytics`

## General Rules

- Side numbers target adjacent blocks.
- Optional tank indexes are `1`-based.
- Inspection of fluid descriptions requires item-stack inspection to be enabled in config.

## API

### getTankLevel

- Syntax: `device.getTankLevel(side[, tank])`
- Returns:
  - Success: `number`
  - Failure: `nil, "no tank"`
- Purpose: Returns the amount of fluid in one adjacent tank.

If `tank` is omitted, the implementation sums the amounts across all tanks on that side.

### getTankCapacity

- Syntax: `device.getTankCapacity(side[, tank])`
- Returns:
  - Success: `number`
  - Failure: `nil, "no tank"`
- Purpose: Returns the capacity of one adjacent tank.

If `tank` is omitted, the implementation returns the maximum capacity among tanks on that side.

### getFluidInTank

- Syntax: `device.getFluidInTank(side[, tank])`
- Returns:
  - Success: tank info table or table array
  - Failure: `nil, reason`
- Purpose: Returns fluid information for one adjacent tank or for all tanks on that side.

Possible failure reasons:

- `no tank`
- `not enabled in config`

### getTankCount

- Syntax: `device.getTankCount(side)`
- Returns:
  - Success: `number`
  - Failure: `nil, "no tank"`
- Purpose: Returns how many tanks are exposed on the specified side.

## Example

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

print("Tank count on top:", transposer.getTankCount(sides.top))
print("Total fluid on top:", transposer.getTankLevel(sides.top))
print("Top tank 1 capacity:", transposer.getTankCapacity(sides.top, 1))

local info = transposer.getFluidInTank(sides.top, 1)
if info and info.fluid then
  print("Fluid amount:", info.fluid.amount)
end
```

## Related

- `opencomputers-tank-world-control`
- `opencomputers-fluid-container-transfer`
- `transposer`
