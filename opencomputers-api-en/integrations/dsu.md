# dsu

## Summary

This page documents the Deep Storage Unit integration exposed to Lua by OpenComputers.

## Availability

- Dependency: `dsu`
- Label: `integration-required`

## Component Names

- `deep_storage_unit`

## What This Integration Is Good For

- Reading the total storage capacity of Deep Storage Units.
- Checking how many items are currently stored.
- Inspecting the stored item type when item stack inspection is enabled in config.

## Components

### deep_storage_unit

`deep_storage_unit` is exposed on tiles implementing the MineFactory Reloaded `IDeepStorageUnit` interface.

#### `getMaxStoredCount()`

- Syntax: `getMaxStoredCount(): number`
- Purpose: Return the maximum number of items the DSU can hold.
- Returns:
  - Maximum stored item count.

#### `getStoredCount()`

- Syntax: `getStoredCount(): number`
- Purpose: Return how many items are currently stored.
- Returns:
  - Current stored item count.
- Usage notes:
  - Returns `0` when no item type is currently stored.

#### `getStoredItemType()`

- Syntax: `getStoredItemType(): table[, string]`
- Purpose: Return the item stack descriptor for the stored item type.
- Returns:
  - An item stack description table when enabled.
  - `nil, "not enabled in config"` if item stack inspection is disabled.
- Usage notes:
  - This callback depends on the OpenComputers config flag `allowItemStackInspection`.
  - If the DSU is currently empty, the callback returns `nil` when inspection is enabled.

## Example

```lua
local component = require("component")
local dsu = component.deep_storage_unit

print("Stored:", dsu.getStoredCount(), "/", dsu.getMaxStoredCount())
local stack, err = dsu.getStoredItemType()
print(stack and stack.label or err)
```

## Related

- `storagedrawers`
- `vanilla`
