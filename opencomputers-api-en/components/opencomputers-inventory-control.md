# opencomputers-inventory-control

## Summary

This page documents the inventory-selection callbacks exposed by inventory-aware hosts such as the `robot` component. These callbacks work on the device's own internal inventory, not on adjacent inventories.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: usually part of `robot` and similar inventory-owning hosts
- Backing trait: `InventoryControl`

## Slot Numbering

Lua-facing slot numbers are `1`-based.

- Slot `1` is the first internal slot.
- Passing an out-of-range slot raises `invalid slot`.

## API

### inventorySize

- Syntax: `device.inventorySize()`
- Returns: `number`
- Purpose: Returns the size of the host's internal inventory.

### select

- Syntax: `device.select([slot])`
- Returns: `number`
- Purpose: Reads or changes the currently selected slot.

When called without an argument, it returns the current selected slot. When a slot is provided, it becomes the new selected slot.

### count

- Syntax: `device.count([slot])`
- Returns: `number`
- Purpose: Returns the stack size in the specified slot, or in the selected slot if omitted.

Empty slots return `0`.

### space

- Syntax: `device.space([slot])`
- Returns: `number`
- Purpose: Returns the remaining item capacity in the specified slot, or in the selected slot if omitted.

For empty slots, this usually returns the inventory stack limit.

### compareTo

- Syntax: `device.compareTo(otherSlot[, checkNBT])`
- Parameters:
  - `otherSlot:number`: slot to compare against.
  - `checkNBT:boolean`: when `true`, includes NBT in the comparison.
- Returns: `boolean`
- Purpose: Compares the selected slot with another internal slot.

Behavior:

- Two empty slots compare as `true`.
- One empty and one non-empty slot compare as `false`.

### transferTo

- Syntax: `device.transferTo(toSlot[, amount])`
- Parameters:
  - `toSlot:number`: destination slot inside the same inventory.
  - `amount:number`: maximum item count to move, clamped to `0..64`.
- Returns: `boolean`
- Purpose: Moves items from the selected slot into another internal slot.

Behavior details:

- Returns `true` immediately if `toSlot` is the selected slot or `amount` is `0`.
- Stacks matching items when possible.
- Swaps stacks only when the full source stack is allowed to move.
- Returns `false` when the move cannot be completed.

## Example

```lua
local robot = require("robot")

print("Inventory size:", robot.inventorySize())
print("Selected slot:", robot.select())

robot.select(1)
print("Items in slot 1:", robot.count())
print("Free space in slot 1:", robot.space())
print("Slot 1 equals slot 2:", robot.compareTo(2, true))

local moved = robot.transferTo(2, 8)
print("Moved items:", moved)
```

## Related

- `opencomputers-inventory-analytics`
- `opencomputers-inventory-world-control`
- `robot`
