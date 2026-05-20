# tmechworks

## Summary

This page documents the Tinkers' Mechworks integration exposed to Lua by OpenComputers.

## Availability

- Dependency: `tmechworks`
- Label: `integration-required`

## Component Names

- `drawbridge`

## What This Integration Is Good For

- Reading whether a drawbridge is currently extended.
- Using drawbridge state in larger automation or safety logic.

## Components

### drawbridge

`drawbridge` is exposed on Tinkers' Mechworks drawbridges.

#### `hasExtended()`

- Syntax: `hasExtended(): boolean`
- Purpose: Return whether the drawbridge is currently extended.
- Returns:
  - `true` if the bridge is extended.
  - `false` otherwise.

## Example

```lua
local component = require("component")
local bridge = component.drawbridge
print("Extended:", bridge.hasExtended())
```

## Related

- `redstone`
- `vanilla`
