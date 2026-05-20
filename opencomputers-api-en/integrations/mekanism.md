# mekanism

## Summary

This page documents the OpenComputers bridge for Mekanism machines that expose strict energy storage.

## Availability

- Dependency: `mekanism`
- Label: `integration-required`

## Component Name

- `mekanism_machine`

## What This Integration Is Good For

- Reading current Mekanism energy reserves.
- Monitoring battery buffers and powered machines.
- Triggering automation when a machine runs low on stored power.

## Component

### mekanism_machine

`mekanism_machine` is exposed on tiles implementing `IStrictEnergyStorage`.

#### `getEnergyStored()`

- Syntax: `getEnergyStored(): number`
- Purpose: Return the current stored energy.
- Returns:
  - Current stored energy value.

#### `getMaxEnergyStored()`

- Syntax: `getMaxEnergyStored(): number`
- Purpose: Return the maximum energy capacity.
- Returns:
  - Maximum stored energy capacity.
- Usage notes:
  - These values come from Mekanism's strict energy API, not from RF wrappers.
  - Both callbacks are read-only and safe to poll regularly.

#### Example

```lua
local component = require("component")
local machine = component.mekanism_machine

local stored = machine.getEnergyStored()
local capacity = machine.getMaxEnergyStored()

print("Energy:", stored, "/", capacity)
```

## Related

- `draconicevolution`
- `cofh`
