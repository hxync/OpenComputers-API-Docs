# opencomputers-inventory-transfer

## Summary

This page documents the transfer callbacks exposed by transposers and similar external inventory-routing devices.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: `transposer`
- Backing trait: `InventoryTransfer`

## Side Rules

These callbacks work on adjacent blocks, not on the device's own inventory.

- Side numbers depend on the host's orientation rules.
- Missing or invalid adjacent inventories return `nil, "no inventory"` for item operations that require them.

## API

### transferItem

- Syntax:
  - `transposer.transferItem(sourceSide, sinkSide[, count[, sourceSlot[, sinkSlot]]])`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Moves items from one adjacent inventory to another.

Return value:

- The number of items actually moved.

Possible failure reasons:

- `no inventory`
- a host-specific transfer lock reason returned by the component's internal guard

Slot arguments are `1`-based.

### swap

- Syntax: `transposer.swap(sourceSide, sinkSide, sourceSlot, sinkSlot[, safe])`
- Returns:
  - Success: `boolean`
  - Failure: `nil, reason`
- Purpose: Swaps two specific slots between adjacent inventories.

`safe` defaults to `false`. When `safe` is `true`, the implementation requires both slots to be non-empty before attempting the swap.

### transferFluid

- Syntax: `transposer.transferFluid(sourceSide, sinkSide[, count[, sourceTank]])`
- Returns:
  - Success: `boolean, number`
  - Failure: `nil, reason`
- Purpose: Moves fluid from one adjacent tank or fluid handler to another.

Return values:

- First value: whether any fluid moved.
- Second value: how many millibuckets moved.

Possible failure reasons:

- `no inventory` from the host-side guard path
- `device has fluid transfer rate of 0`
- a host-specific transfer lock reason

`count` defaults to one bucket, and `sourceTank` is `1`-based when supplied.

### getFluidTransferRate

- Syntax: `transposer.getFluidTransferRate()`
- Returns: `number`
- Purpose: Returns the device's configured fluid transfer rate in liters per second.

## Example

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer

local moved, reason = transposer.transferItem(sides.north, sides.south, 16, 1, 1)
if moved then
  print("Items moved:", moved)
else
  print("Item transfer failed:", reason)
end

local ok, amount = transposer.transferFluid(sides.west, sides.east, 1000, 1)
print("Fluid moved:", ok, amount)
print("Fluid rate:", transposer.getFluidTransferRate())
```

## Related

- `opencomputers-fluid-container-transfer`
- `opencomputers-world-inventory-analytics`
- `opencomputers-world-tank-analytics`
