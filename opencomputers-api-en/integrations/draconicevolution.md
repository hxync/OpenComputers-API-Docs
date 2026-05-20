# draconicevolution

## Summary

This page documents the OpenComputers bridge for Draconic Evolution energy storage blocks that implement the extended RF storage API.

## Availability

- Dependency: `draconicevolution`
- Label: `integration-required`

## Component Name

- `draconic_storage`

## What This Integration Is Good For

- Reading current and maximum RF storage.
- Building large-capacity power dashboards.
- Triggering automation when a Draconic buffer becomes full or empty.

## Component

### draconic_storage

`draconic_storage` is exposed on tiles that implement `IExtendedRFStorage`.

#### `getEnergyStored()`

- Syntax: `getEnergyStored(): number`
- Purpose: Return the current amount of stored energy.
- Returns:
  - Current stored energy.

#### `getMaxEnergyStored()`

- Syntax: `getMaxEnergyStored(): number`
- Purpose: Return the maximum energy capacity.
- Returns:
  - Maximum stored energy capacity.
- Usage notes:
  - Both callbacks are read-only.
  - The values come directly from the tile's energy storage implementation.
  - The reported unit is the RF-style energy value used by the target block.

#### Example

```lua
local component = require("component")
local storage = component.draconic_storage

local stored = storage.getEnergyStored()
local capacity = storage.getMaxEnergyStored()

print("Energy:", stored, "/", capacity)
print(string.format("Fill: %.2f%%", stored / capacity * 100))
```

## Related

- `mekanism`
- `cofh`
