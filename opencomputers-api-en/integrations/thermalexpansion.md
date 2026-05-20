# thermalexpansion

## Summary

This page documents the Thermal Expansion integration exposed to Lua by OpenComputers. The current integration provides color control for compatible Thermal Expansion lamps.

## Availability

- Dependency: `thermalexpansion`
- Label: `integration-required`

## Component Names

- `lamp`

## What This Integration Is Good For

- Changing the color of Thermal Expansion lamps from scripts.
- Building status displays, warning lights, or decorative color patterns driven by automation logic.

## Components

### lamp

`lamp` is exposed on compatible Thermal Expansion lamp tiles.

#### `setColor(color)`

- Syntax: `setColor(color: number): boolean`
- Purpose: Change the lamp color.
- Parameters:
  - `color: number`
    Integer color value passed directly to the Thermal Expansion lamp's `setColor(int)` method.
- Returns:
  - Whatever the underlying `setColor` method returns.
  - If reflection fails, the method is missing, or the target tile rejects the call, this becomes `nil`.
- Usage notes:
  - OpenComputers does not clamp, translate, or reinterpret this number. It forwards the integer exactly as provided.
  - This is not the OC GPU `0xRRGGBB` color format. Use the lamp's own color encoding instead.

## Example

```lua
local component = require("component")

if component.isAvailable("lamp") then
  local lamp = component.lamp
  lamp.setColor(3)
end
```

## Related

- `cofh`
- `redstone`
