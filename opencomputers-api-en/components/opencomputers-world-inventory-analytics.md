# opencomputers-world-inventory-analytics

## Summary

This page documents read-only and metadata-oriented inspection callbacks for adjacent inventories.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: commonly exposed by `transposer` and `inventory_controller`
- Backing trait: `WorldInventoryAnalytics`

## General Rules

- Side numbers target adjacent inventories.
- Slot numbers are `1`-based.
- Some callbacks require the item-stack inspection config to be enabled.
- Invalid database addresses raise errors such as `no such component` or `not a database`.

## API

### getInventorySize

- Syntax: `device.getInventorySize(side)`
- Returns:
  - Success: `number`
  - Failure: `nil, "no inventory"`
- Purpose: Returns the number of slots in the adjacent inventory.

### getSlotStackSize

- Syntax: `device.getSlotStackSize(side, slot)`
- Returns:
  - Success: `number`
  - Failure: `nil, "no inventory"`
- Purpose: Returns the stack size in one adjacent inventory slot.

### getSlotMaxStackSize

- Syntax: `device.getSlotMaxStackSize(side, slot)`
- Returns:
  - Success: `number`
  - Failure: `nil, "no inventory"`
- Purpose: Returns the maximum stack size of the current item in one adjacent slot.

Empty slots return `0`.

### compareStacks

- Syntax: `device.compareStacks(side, slotA, slotB[, checkNBT])`
- Returns:
  - Success: `boolean`
  - Failure: `nil, "no inventory"`
- Purpose: Checks whether two slots in the same adjacent inventory hold the same item type.

### compareStackToDatabase

- Syntax: `device.compareStackToDatabase(side, slot, dbAddress, dbSlot[, checkNBT])`
- Returns:
  - Success: `boolean`
  - Failure: `nil, "no inventory"`
- Purpose: Compares one adjacent inventory slot with a database entry.

### areStacksEquivalent

- Syntax: `device.areStacksEquivalent(side, slotA, slotB)`
- Returns:
  - Success: `boolean`
  - Failure: `nil, "no inventory"`
- Purpose: Checks OreDictionary equivalence between two adjacent inventory slots.

### setStackDisplayName

- Syntax: `device.setStackDisplayName(side, slot, label)`
- Returns:
  - Success: `boolean`
  - Failure: `nil, "no inventory"`
- Purpose: Sets or clears the display name of one adjacent inventory stack.

Passing an empty or whitespace-only `label` clears an existing custom name.

### getStackInSlot

- Syntax: `device.getStackInSlot(side, slot)`
- Returns:
  - Success: item description table
  - Failure: `nil, reason`
- Purpose: Returns a detailed description of one adjacent inventory slot.

### getAllStacks

- Syntax: `device.getAllStacks(side)`
- Returns:
  - Success: userdata iterator-like stack array wrapper
  - Failure: `nil, reason`
- Purpose: Returns a bulk view of all stacks in the adjacent inventory.

### getInventoryName

- Syntax: `device.getInventoryName(side)`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Returns an identifier for the adjacent inventory source.

Typical results:

- block unlocalized name for block inventories
- entity string ID for entity-backed inventories

### store

- Syntax: `device.store(side, slot, dbAddress, dbSlot)`
- Returns:
  - Success: `boolean`
  - Failure: `nil, "no inventory"`
- Purpose: Copies one adjacent inventory slot into a database slot.

Return value:

- `true` if the destination database slot already had data.
- `false` if it was previously empty.

## Common Inspection Failures

Inspection-gated callbacks may also return:

- `nil, "not enabled in config"`

## Example

```lua
local component = require("component")
local sides = require("sides")
local transposer = component.transposer
local db = component.database

print("Front inventory size:", transposer.getInventorySize(sides.front))
print("Slot 1 stack size:", transposer.getSlotStackSize(sides.front, 1))
print("Slot 1 equals slot 2:", transposer.compareStacks(sides.front, 1, 2, true))

local stack = transposer.getStackInSlot(sides.front, 1)
if stack then
  print("Slot 1 item:", stack.name)
end

transposer.store(sides.front, 1, db.address, 1)
print("Database match:", transposer.compareStackToDatabase(sides.front, 1, db.address, 1, true))
```

## Related

- `opencomputers-inventory-transfer`
- `opencomputers-world-fluid-container-analytics`
- `database`
