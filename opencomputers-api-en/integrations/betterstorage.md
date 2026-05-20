# betterstorage

## Summary

This page documents the BetterStorage crate integration. Crates become Lua components that can enumerate their contents, and newer BetterStorage API versions also expose crate capacity.

## Availability

- Dependency: `betterstorage`
- Label: `integration-required`

## Component Name

- `crate`

## What This Integration Is Good For

- Listing every stack currently stored in a crate.
- Building inventory browsers for crate-based warehouses.
- Estimating free storage space when the newer BetterStorage crate API is present.

## Components

OpenComputers supports two BetterStorage crate driver variants because the mod shipped different crate APIs across versions. Both expose the same Lua component name.

### crate

#### `getContents()`

- Syntax: `getContents(): table`
- Purpose: Return the crate contents as an array of item stack tables.
- Returns:
  - A snapshot array of item stack descriptor tables.
- Usage notes:
  - Each returned stack is the usual OpenComputers item-stack structure, typically including item name, label, damage, stack size, and NBT-related metadata when available.
  - `getContents()` returns a snapshot, not a live iterator.
  - The order of returned stacks follows the crate's internal content list and should not be treated as a stable sort order.

#### `getCapacity()`

- Syntax: `getCapacity(): number`
- Purpose: Return how many item slots the crate can hold.
- Returns:
  - Crate capacity in slots.
- Usage notes:
  - This callback only exists on the newer BetterStorage API.
  - Code that targets multiple modpack versions should check whether the method exists before calling it.

#### Example

```lua
local component = require("component")
local crate = component.crate

if crate.getCapacity then
  print("Capacity:", crate.getCapacity())
end

for index, stack in ipairs(crate.getContents()) do
  print(index, stack.label or stack.name, stack.size)
end
```

## Related

- `storagedrawers`
- `opencomputers-inventory-controller`
