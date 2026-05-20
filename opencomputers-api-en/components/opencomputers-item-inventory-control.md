# opencomputers-item-inventory-control

## Summary

This page documents callbacks for inventories contained inside items, such as boxes, bags, or other item-based storage that OpenComputers can open through the installed driver.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: `inventory_controller`
- Backing trait: `ItemInventoryControl`

## Slot Numbering

- The host inventory slot containing the item inventory is `1`-based.
- The slot inside the item inventory is also `1`-based.

## API

### getItemInventorySize

- Syntax: `inventory_controller.getItemInventorySize(slot)`
- Returns:
  - Success: `number`
  - Failure: `0, "no item inventory"`
- Purpose: Returns the number of slots exposed by the item inventory in the specified host slot.

### dropIntoItemInventory

- Syntax: `inventory_controller.dropIntoItemInventory(inventorySlot, slot[, count])`
- Returns:
  - Success: `number`
  - Failure: `0, "no item inventory"`
- Purpose: Moves items from the host inventory into a slot inside an item inventory.

The return value is the number of items actually moved.

### suckFromItemInventory

- Syntax: `inventory_controller.suckFromItemInventory(inventorySlot, slot[, count])`
- Returns:
  - Success: `number`
  - Failure: `0, "no item inventory"`
- Purpose: Pulls items from a slot inside an item inventory back into the host inventory.

## Example

```lua
local component = require("component")
local ic = component.inventory_controller

local size, reason = ic.getItemInventorySize(3)
if size == 0 and reason then
  print("Slot 3 does not contain an item inventory:", reason)
else
  print("Contained inventory size:", size)
  print("Moved into contained slot:", ic.dropIntoItemInventory(3, 1, 16))
  print("Moved out of contained slot:", ic.suckFromItemInventory(3, 1, 16))
end
```

## Related

- `opencomputers-inventory-analytics`
- `opencomputers-inventory-world-control-mk2`
- `inventory_controller`
