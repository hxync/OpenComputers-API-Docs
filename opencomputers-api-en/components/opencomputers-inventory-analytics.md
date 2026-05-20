# opencomputers-inventory-analytics

## Summary

This page documents internal-inventory inspection callbacks. They are typically exposed by the `inventory_controller` upgrade on robots and drones.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: `inventory_controller`
- Backing trait: `InventoryAnalytics`

## Slot Numbering

Lua-facing inventory slots are `1`-based.

## API

### getStackInInternalSlot

- Syntax: `inventory_controller.getStackInInternalSlot([slot])`
- Returns:
  - Success: item description table
  - Failure: `nil, "not enabled in config"`
- Purpose: Returns a detailed stack description for the specified internal slot, or for the selected slot if omitted.

This callback is gated by the OpenComputers item-stack inspection config option.

### isEquivalentTo

- Syntax: `inventory_controller.isEquivalentTo(otherSlot)`
- Returns: `boolean`
- Purpose: Checks whether the selected slot and another internal slot share at least one OreDictionary entry.

Behavior:

- Two empty slots count as equivalent.
- Empty versus non-empty counts as not equivalent.

### storeInternal

- Syntax: `inventory_controller.storeInternal(slot, dbAddress, dbSlot)`
- Returns: `boolean`
- Purpose: Copies the selected internal stack description into a database slot.

Return value:

- `true` if the destination database slot already contained data.
- `false` if the destination database slot was previously empty.

Important note:

- The implementation expects a real stack in the source slot. Empty source slots are not a valid use case.
- Invalid database addresses raise errors such as `no such component` or `not a database`.

### compareToDatabase

- Syntax: `inventory_controller.compareToDatabase(slot, dbAddress, dbSlot[, checkNBT])`
- Returns: `boolean`
- Purpose: Compares one internal slot against an item record stored in a database.

`checkNBT` defaults to `false`.

## Example

```lua
local component = require("component")
local robot = require("robot")
local ic = component.inventory_controller
local db = component.database

robot.select(1)

local stack, reason = ic.getStackInInternalSlot()
if stack then
  print("Selected item:", stack.name, stack.size)
else
  print("Inspection unavailable:", reason)
end

ic.storeInternal(1, db.address, 1)
print("Matches database slot 1:", ic.compareToDatabase(1, db.address, 1, true))
print("Equivalent to slot 2:", ic.isEquivalentTo(2))
```

## Related

- `opencomputers-inventory-control`
- `opencomputers-world-inventory-analytics`
- `database`
