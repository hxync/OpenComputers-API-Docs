# chunkloader

## Summary

The `chunkloader` component is the control surface of the chunk loader upgrade. It keeps the host device's current chunk and the surrounding eight chunks loaded while active, allowing robots, microcontrollers, and similar devices to keep running in unloaded areas.

## Availability

- Repository: `opencomputers`
- Lua component name: `chunkloader`
- Typical hosts: robots, microcontrollers, and other devices that can install the chunk loader upgrade

## What It Does

- Holds a `3 x 3` chunk area centered on the host's current chunk.
- Follows the host as it moves.
- Starts automatically when the computer starts and stops automatically when the computer stops.
- Shuts itself off if the device can no longer pay the ongoing chunk loading energy cost.

## Activation Rules

- The upgrade cannot be enabled in dimensions blocked by the OpenComputers chunk-loading whitelist or blacklist.
- If Lua calls `setActive(true)` in a blocked dimension, the call raises an error.
- If power runs out after activation, the upgrade releases its ticket and `isActive()` becomes `false`.

## API

### isActive

- Syntax: `chunkloader.isActive()`
- Returns: `boolean`
- Purpose: Checks whether the upgrade currently holds an active Forge chunk-loading ticket.

### setActive

- Syntax: `chunkloader.setActive(enabled)`
- Parameters:
  - `enabled:boolean`: `true` to request chunk loading, `false` to release it.
- Returns: `boolean`
- Purpose: Enables or disables the upgrade manually.

Return value meanings:

- `true`: the active state changed.
- `false`: the requested state was already in effect.

Possible failure:

- Raises an error with the message `this dimension is blacklisted` if activation is requested in a blocked dimension.

## Practical Notes

- On mobile hosts such as robots, the loaded area moves with the host.
- The upgrade stores its chunk ticket and can restore it after world reload if the host reconnects normally.
- Drones and other entity-based hosts update their loaded area by periodic polling instead of move events, but the end result is the same from a Lua perspective.

## Example

```lua
local component = require("component")
local loader = component.chunkloader

if not loader.isActive() then
  local ok, changed = pcall(loader.setActive, true)
  if not ok then
    error("chunkloader could not be enabled: " .. tostring(changed))
  end
  print("State changed:", changed)
end

print("Chunkloader active:", loader.isActive())
```

## Related

- `robot`
- `microcontroller`
- `computer`
