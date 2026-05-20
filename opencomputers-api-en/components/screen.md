# screen

## Summary

The `screen` component is the text-buffer interface exposed by screens. It controls screen power state, keyboard association, precision pointer reporting, and multi-block geometry as seen by the connected GPU/text system.

## Availability

- Repository: `opencomputers`
- Lua component name: `screen`
- Typical hosts: screen blocks and screen-backed text buffers

## What It Does

- Reports whether the screen is on.
- Turns the screen on or off.
- Reports the screen's aspect ratio in blocks.
- Lists attached keyboards.
- Gets or sets precision mouse mode on tier 3 capable screens.

## Multi-Block Notes

- Multi-block screens merge only when tier, facing, and dye color are compatible.
- The aspect ratio is reported as block width and block height, not pixel resolution.
- Resolution itself is controlled through the bound GPU, not through this component.

## API

### isOn

- Syntax: `screen.isOn()`
- Returns: `boolean`
- Purpose: Returns whether the screen is currently powered on for display.

### turnOn

- Syntax: `screen.turnOn()`
- Returns: `changed, isOn`
- Purpose: Turns the screen on.

Return values:

- `changed:boolean`: whether the power state changed.
- `isOn:boolean`: the final on/off state after the call.

### turnOff

- Syntax: `screen.turnOff()`
- Returns: `changed, isOn`
- Purpose: Turns the screen off.

Return values use the same meaning as `turnOn()`.

### getAspectRatio

- Syntax: `screen.getAspectRatio()`
- Returns: `widthBlocks, heightBlocks`
- Purpose: Returns the screen's block dimensions as currently merged.

### getKeyboards

- Syntax: `screen.getKeyboards()`
- Returns: `table`
- Purpose: Returns a list of addresses for keyboards attached to this screen.

Behavior:

- The call pauses briefly before returning.
- On a multi-block screen, it aggregates keyboards from all merged screen blocks.

### isPrecise

- Syntax: `screen.isPrecise()`
- Returns: `boolean`
- Purpose: Returns whether mouse events use high precision coordinates.

In precise mode, pointer events use fractional coordinates instead of whole character cells.

### setPrecise

- Syntax: `screen.setPrecise(enabled)`
- Parameters:
  - `enabled:boolean`: desired precision mode.
- Returns:
  - Success: previous precision mode as `boolean`
  - Failure: `nil, "unsupported operation"`
- Purpose: Enables or disables high precision pointer reporting.

Only tier 3 capable screens support this operation.

## Practical Notes

- Pointer events are sent as `touch`, `drag`, `drop`, and `scroll` signals.
- When precision mode is disabled, coordinates are reported as 1-based character cells.
- When precision mode is enabled, coordinates are reported as floating point values in viewport space.

## Example

```lua
local component = require("component")
local screen = component.screen

local changed, isOn = screen.turnOn()
print("Changed:", changed, "On:", isOn)

local w, h = screen.getAspectRatio()
print("Block size:", w, h)

print("Attached keyboards:", table.concat(screen.getKeyboards(), ", "))

local old, reason = screen.setPrecise(true)
print("Previous precise mode:", old, reason)
print("Precise mode now:", screen.isPrecise())
```

## Related

- `opencomputers-screen`
- `gpu`
- `keyboard`
