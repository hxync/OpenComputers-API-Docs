# opencomputers-fluid-container-transfer

## Summary

This page documents the callbacks that move fluid between tank blocks and fluid containers stored in inventories. These functions are primarily exposed by the `transposer` component.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: `transposer`
- Backing trait: `FluidContainerTransfer`

## General Rules

- Inventory slots are `1`-based.
- Fluid amounts are in millibuckets.
- `count` defaults to one bucket when omitted.
- Missing adjacent inventories return `nil, "no inventory"`.

## API

### transferFluidFromTankToContainer

- Syntax: `transposer.transferFluidFromTankToContainer(tankSide, inventorySide, inventorySlot[, count[, sourceTank[, outputSide[, outputSlot]]]])`
- Returns:
  - Success: `boolean, number`
  - Failure: `nil, reason`
- Purpose: Fills a fluid container item from a tank or fluid handler on the specified side.

Return values:

- First value indicates whether any fluid moved.
- Second value is the moved amount in millibuckets.

Parameters:

- `tankSide:number`: side of the source tank block.
- `inventorySide:number`: side of the inventory holding the container item.
- `inventorySlot:number`: slot containing the source container item.
- `count:number`: maximum amount to move.
- `sourceTank:number`: optional source tank index, `1`-based.
- `outputSide:number`: optional side of the inventory that should receive the resulting container item.
- `outputSlot:number`: optional exact output slot.

### transferFluidFromContainerToTank

- Syntax: `transposer.transferFluidFromContainerToTank(inventorySide, inventorySlot, tankSide[, count[, outputSide[, outputSlot]]])`
- Returns:
  - Success: `boolean, number`
  - Failure: `nil, reason`
- Purpose: Empties a fluid container item into an adjacent tank or fluid handler.

### transferFluidBetweenContainers

- Syntax: `transposer.transferFluidBetweenContainers(sourceSide, sourceSlot, sinkSide, sinkSlot[, count[, sourceOutputSide[, sinkOutputSide[, sourceOutputSlot[, sinkOutputSlot]]]]])`
- Returns:
  - Success: `boolean, number`
  - Failure: `nil, reason`
- Purpose: Moves fluid from one inventory-held fluid container into another.

This operation only commits if both resulting container items can be placed back into their designated output locations.

## Practical Notes

- If an output side or output slot is omitted, the processed container item usually goes back to the source inventory.
- The implementation simulates the output placement before committing some multi-container moves, which prevents partially applied conversions.
- Host-specific transfer guards may return additional failure reasons.

## Example

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local ok, amount = transposer.transferFluidFromTankToContainer(
  sides.left,   -- tank
  sides.right,  -- inventory
  1,            -- container slot
  1000,
  1
)
print("Filled container:", ok, amount)

local emptied, moved = transposer.transferFluidFromContainerToTank(
  sides.right,
  2,
  sides.left,
  500
)
print("Drained container:", emptied, moved)
```

## Related

- `opencomputers-inventory-transfer`
- `opencomputers-world-fluid-container-analytics`
- `opencomputers-world-tank-analytics`
