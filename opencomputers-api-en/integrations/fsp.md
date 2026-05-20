# fsp

## Summary

This page documents the Flaxbeard's Steam Power integration exposed through OpenComputers and Computronics. It adds one read-only component for inspecting the state of steam transport blocks.

## Availability

- Dependency: `fsp`
- Label: `integration-required`

## Component Name

- `steam_transporter`

## What This Integration Is Good For

- Monitoring whether a steam line is full, empty, or pressurized.
- Triggering alarms when boilers stop supplying steam.
- Feeding live steam telemetry into dashboards or control loops.

## Component

### steam_transporter

`steam_transporter` is exposed on blocks implementing the FSP `ISteamTransporter` interface.

#### getSteamPressure()

- Syntax: `getSteamPressure(): number`
- Purpose: Return the current steam pressure reported by the transporter.
- Returns:
  - The current pressure value from `getPressure()`.
- Usage notes:
  - This is a read-only query.
  - The value is returned directly from the underlying transporter implementation without additional scaling by the OC driver.

#### getSteamCapacity()

- Syntax: `getSteamCapacity(): number`
- Purpose: Return the maximum amount of steam the transporter can hold.
- Returns:
  - The capacity value from `getCapacity()`.
- Usage notes:
  - This is a read-only query.

#### getSteamAmount()

- Syntax: `getSteamAmount(): number`
- Purpose: Return how much steam is currently stored in the transporter.
- Returns:
  - The stored steam amount from `getSteam()`.
- Usage notes:
  - This is a read-only query.
  - Pairing this with `getSteamPressure()` is the most practical way to detect empty lines, stalls, or backups.

## Example

```lua
local component = require("component")
local transporter = component.steam_transporter

local pressure = transporter.getSteamPressure()
local amount = transporter.getSteamAmount()
local capacity = transporter.getSteamCapacity()

print(string.format("Steam: %d / %d", amount, capacity))
print("Pressure:", pressure)

if amount == 0 then
  print("Warning: the line is empty.")
end
```

## Related

- `railcraft`
- `buildcraft`
