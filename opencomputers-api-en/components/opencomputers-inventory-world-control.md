# opencomputers-inventory-world-control

## Summary

This page documents the classic world-interaction inventory callbacks used by the `robot` component and related agent-like devices.

## Availability

- Repository: `opencomputers`
- Typical Lua component name: usually part of `robot` or agent-style components
- Backing trait: `InventoryWorldControl`

## API

### compare

- Syntax: `device.compare(side[, fuzzy])`
- Returns: `boolean`
- Purpose: Compares the block on the specified side with the item currently in the selected slot.

How it works:

- It only succeeds for block items.
- Item ID must match the target block.
- Metadata must also match unless `fuzzy` is `true`.

### drop

- Syntax: `device.drop(side[, count])`
- Returns:
  - Success: `true`
  - Failure: `false` or `false, "inventory full"`
- Purpose: Drops items from the selected slot toward the specified side.

Behavior:

- If an adjacent inventory is available and usable, the device inserts into that inventory first.
- Otherwise it ejects the items into the world as dropped entities.
- `count` defaults to `64`.

### suck

- Syntax: `device.suck(side[, count])`
- Returns:
  - Success: `number`
  - Failure: `false`
- Purpose: Pulls items from the specified side.

Behavior:

- Tries an adjacent inventory first.
- If no items can be extracted, tries to pick up dropped item entities on that side.
- Returns the number of items actually collected.

## Practical Notes

- `drop` and `suck` both pause execution using the normal OpenComputers action delay settings.
- `suck` can return `false` when nothing was found, not `0`.

## Example

```lua
local robot = require("robot")
local sides = require("sides")

robot.select(1)
print("Front block matches selected stack:", robot.compare(sides.front, true))

local ok, reason = robot.drop(sides.front, 16)
print("Drop result:", ok, reason)

local taken = robot.suck(sides.front, 8)
print("Items collected:", taken)
```

## Related

- `opencomputers-inventory-world-control-mk2`
- `opencomputers-inventory-control`
- `robot`
