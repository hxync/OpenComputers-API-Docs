# gc

## Summary

This page documents the Galacticraft world sensor integration exposed to Lua by OpenComputers.

## Availability

- Dependency: `gc`
- Label: `integration-required`

## Component Names

- `world_sensor`

## What This Integration Is Good For

- Reading gravity and wind values in Galacticraft dimensions.
- Checking whether the current world has a breathable atmosphere.
- Testing whether a specific atmospheric gas is present.

## Components

### world_sensor

`world_sensor` is exposed by the Galacticraft world sensor card.

#### `getGravity()`

- Syntax: `getGravity(): number`
- Purpose: Return the gravity value of the current world.
- Returns:
  - Galacticraft gravity multiplier for the current dimension.
- Usage notes:
  - If the host world is not a Galacticraft world provider, this falls back to `1`.

#### `getWindLevel()`

- Syntax: `getWindLevel(): number`
- Purpose: Return the wind level of the current world.
- Returns:
  - Wind level for the current dimension.
- Usage notes:
  - If the host world is not a Galacticraft world provider, this falls back to `1`.

#### `hasBreathableAtmosphere()`

- Syntax: `hasBreathableAtmosphere(): boolean`
- Purpose: Return whether the current world has a breathable atmosphere.
- Returns:
  - `true` if breathing is possible without extra support.
  - `false` otherwise.
- Usage notes:
  - Non-Galacticraft worlds default to `true`.

#### `isGasPresent(gas)`

- Syntax: `isGasPresent(gas: string): boolean`
- Purpose: Return whether the current world atmosphere contains the specified gas.
- Parameters:
  - `gas: string`
    Atmospheric gas name such as `oxygen` or `nitrogen`.
- Returns:
  - `true` if the gas is present.
  - `false` otherwise.
- Usage notes:
  - The string is converted to uppercase before matching Galacticraft's gas enum.
  - Non-Galacticraft worlds default to `true`.

## Example

```lua
local component = require("component")
local sensor = component.world_sensor

print("Gravity:", sensor.getGravity())
print("Wind:", sensor.getWindLevel())
print("Breathable:", sensor.hasBreathableAtmosphere())
print("Has oxygen:", sensor.isGasPresent("oxygen"))
```

## Related

- `stargatetech2`
- `vanilla`
