# opencomputers-inventory-world-control-mk2

## Summary

This page documents the slot-precise external inventory callbacks exposed by `inventory_controller`.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: `inventory_controller`
- Backing trait: `InventoryWorldControlMk2`

## Slot Numbering

All inventory slot arguments are `1`-based.

## API

### dropIntoSlot

- Syntax: `inventory_controller.dropIntoSlot(facing, slot[, count[, fromSide]])`
- Returns:
  - Success: `true`
  - Failure: `false, "inventory full/invalid slot"` or `nil, "no inventory"`
- Purpose: Inserts items from the selected internal slot into one exact slot of an adjacent inventory.

Parameters:

- `facing:number`: which adjacent inventory to target.
- `slot:number`: exact slot inside that inventory.
- `count:number`: maximum items to insert, default `64`.
- `fromSide:number`: optional side of the target inventory to insert through. Defaults to the opposite of `facing`.

### suckFromSlot

- Syntax: `inventory_controller.suckFromSlot(facing, slot[, count[, fromSide]])`
- Returns:
  - Success: `number`
  - Failure: `false` or `nil, "no inventory"`
- Purpose: Extracts items from one exact slot of an adjacent inventory into the host inventory.

Parameters use the same meaning as `dropIntoSlot`.

## Example

```lua
local component = require("component")
local robot = require("robot")
local sides = require("sides")
local ic = component.inventory_controller

robot.select(1)

local ok, reason = ic.dropIntoSlot(sides.front, 3, 8)
print("Drop into slot result:", ok, reason)

local moved = ic.suckFromSlot(sides.front, 5, 16)
print("Items taken from external slot:", moved)
```

## Related

- `opencomputers-inventory-world-control`
- `opencomputers-world-inventory-analytics`
- `inventory_controller`
