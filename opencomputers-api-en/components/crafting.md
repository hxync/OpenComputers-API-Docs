# crafting

## Summary

The `crafting` component provides the robot crafting upgrade API. It lets a robot craft items from the `3 x 3` crafting grid mapped onto the top-left portion of the robot inventory.

This component is intentionally small, but its behavior matters: it repeatedly crafts the same recipe until the requested amount is reached, no recipe is available anymore, or the output would change.

## Availability

- Repository: `opencomputers`
- Component name: `crafting`
- Typical host: robot with a crafting upgrade installed

## Usage

```lua
local component = require("component")
local crafting = component.crafting

if not crafting then
  error("crafting upgrade not installed")
end
```

## Inventory Layout

The crafting upgrade reads the robot inventory as a `3 x 3` crafting matrix using the top-left part of the inventory:

```text
slot 1  slot 2  slot 3
slot 5  slot 6  slot 7
slot 9  slot 10 slot 11
```

In other words, each crafting row uses the first three slots of a four-slot robot inventory row.

## API

### craft

- Syntax: `crafting.craft([count])`
- Returns: `boolean, number`
- Purpose: Repeatedly crafts the current recipe from the robot crafting grid.

Parameters:

- `count:number` optional: desired number of output items. Defaults to `64`.

Behavior:

- The requested count is clamped to the range `0` through `64`.
- The component first determines the recipe currently present in the grid.
- It keeps crafting while:
  - enough ingredients remain,
  - the recipe is still valid,
  - the produced output item stays the same.
- Container items and crafting by-products are placed back into the robot inventory when possible.

Return values:

- First result: `true` when a valid recipe existed, otherwise `false`.
- Second result: total number of output items actually crafted.

Examples:

Craft as many items as possible up to a full stack:

```lua
local ok, crafted = crafting.craft()
if ok then
  print("Crafted items:", crafted)
else
  print("No valid recipe in the crafting grid")
end
```

Craft exactly one recipe result when possible:

```lua
local ok, crafted = crafting.craft(1)
print("Recipe found:", ok)
print("Items crafted:", crafted)
```

## Related

- `inventory_controller`
- `robot`
- `component`
