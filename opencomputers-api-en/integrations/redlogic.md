# redlogic

## Summary

This page documents the OpenComputers integration for RedLogic lamps.

## Availability

- Dependency: `redlogic`
- Label: `integration-required`

## Component Name

- `lamp`

## What This Integration Is Good For

- Inspecting lamp color from in-world automation.
- Detecting whether a lamp is currently powered.
- Distinguishing between different RedLogic lamp variants.

## Component

### lamp

The `lamp` component is exposed on blocks implementing `ILampBlock`.

#### `getLampColor()`

- Syntax: `getLampColor(): number`
- Purpose: Return the lamp color as a packed RGB integer.
- Returns:
  - The current lamp color value.
- Usage notes:
  - You will usually want to convert this to hexadecimal for display.

#### `isLampPowered()`

- Syntax: `isLampPowered(): boolean`
- Purpose: Return whether the lamp is currently powered.
- Returns:
  - `true` if the lamp is powered.
  - `false` otherwise.

#### `getLampType()`

- Syntax: `getLampType(): string`
- Purpose: Return the lamp type name reported by RedLogic.
- Returns:
  - The internal lamp type enum name.
- Usage notes:
  - These callbacks only inspect lamp state. They do not switch the lamp on or off.

#### Example

```lua
local component = require("component")
local lamp = component.lamp

print("Type:", lamp.getLampType())
print(string.format("Color: 0x%06X", lamp.getLampColor()))
print("Powered:", lamp.isLampPowered())
```

## Related

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-bundled`
