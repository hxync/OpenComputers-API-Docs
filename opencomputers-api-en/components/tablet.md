# tablet

## Summary

This page documents the real `tablet` component callbacks. They expose the orientation of the player currently holding the tablet, which is useful for upgrades whose behavior depends on the holder's facing.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `tablet`
- Backing implementation: `li.cil.oc.server.component.Tablet`
- Typical host: tablet itself

## What It Does

- Reads the holder's vertical look angle.
- Reads the holder's horizontal look angle.

These values are especially useful for upgrades installed in tablets that act relative to the player's orientation.

## Important Tablet Behavior

- Tablets are portable computers, but they do not persist across logout/login or dimension changes the way placed computers do.
- Shift-right-clicking a Tier 2 tablet opens the alternate container GUI and forcibly stops the tablet.
- Holding right-click on a block for roughly half a second triggers the tablet analysis workflow; upgrades may attach data to the resulting `tablet_use` signal.

Those behaviors are not callbacks of the `tablet` component itself, but they matter when designing tablet software.

## API

### getPitch

- Syntax: `tablet.getPitch()`
- Returns: `number`
- Purpose: Returns the current pitch of the player holding the tablet.

Interpretation:

- Negative values mean the player is looking upward.
- Positive values mean the player is looking downward.

### getYaw

- Syntax: `tablet.getYaw()`
- Returns: `number`
- Purpose: Returns the current yaw of the player holding the tablet.

Interpretation:

- This is the holder's horizontal facing angle in Minecraft rotation degrees.

## Example

```lua
local component = require("component")
local tablet = component.tablet

print("Yaw:", tablet.getYaw())
print("Pitch:", tablet.getPitch())
```

## Related

- `opencomputers-upgrade-navigation`
- `piston`
- `sign`
