# avaritiaaddons

## Summary

This page documents the Avaritia Addons integration exposed to Lua by OpenComputers. The current integration focuses on the Extreme Autocrafter's ghost recipe grid.

## Availability

- Dependency: `avaritiaaddons`
- Label: `integration-required`

## Component Names

- `extreme_autocrafter`

## What This Integration Is Good For

- Reading the ghost recipe items stored in an Extreme Autocrafter.
- Writing recipe ghost items from a database into the autocrafter grid.
- Automating recipe setup without opening the machine GUI manually.

## Components

### extreme_autocrafter

`extreme_autocrafter` is exposed on Avaritia Addons Extreme Autocrafter blocks.

#### `getGhostItem(slot)`

- Syntax: `getGhostItem(slot: number): table`
- Purpose: Return the ghost item configured in one recipe slot.
- Parameters:
  - `slot: number`
    Ghost slot index from `0` through `80`.
- Returns:
  - An item stack description table for the ghost item in that slot.
- Usage notes:
  - Slots outside `0` through `80` raise an argument error.
  - The driver offsets this range into the autocrafter's internal ghost inventory.

#### `setGhostItem(slot, database, databaseSlot[, size])`

- Syntax: `setGhostItem(slot: number, database: string, databaseSlot: number[, size: number]): boolean`
- Purpose: Copy an item from an OpenComputers database into one ghost recipe slot.
- Parameters:
  - `slot: number`
    Ghost slot index from `0` through `80`.
  - `database: string`
    Component address of the database.
  - `databaseSlot: number`
    One-based database slot index.
  - `size: number`
    Optional stack size to write. Defaults to `1`.
- Returns:
  - `true` on success.
- Usage notes:
  - If the database slot is empty or the requested size is less than `1`, the slot is effectively cleared.
  - The copied stack size is clamped to the source item's maximum stack size.
  - Invalid component addresses or non-database components return argument errors.

## Example

```lua
local component = require("component")
local crafter = component.extreme_autocrafter
local db = component.database

crafter.setGhostItem(0, db.address, 1, 1)
local ghost = crafter.getGhostItem(0)
print(ghost and ghost.label)
```

## Related

- `vanilla`
- `appeng`
