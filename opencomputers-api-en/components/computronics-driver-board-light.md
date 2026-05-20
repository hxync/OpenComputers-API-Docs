# light_board

## Summary

The `light_board` component controls a rack-mounted indicator board from Computronics. Each light can be assigned its own color and on/off state, making the board useful for dashboards, rack status displays, alarms, and compact control panels.

The board layout is not fixed. Depending on the hardware mode selected on the rack block, the board exposes a different number of light slots.

## Availability

- Repository: `computronics`
- Component name: `light_board`
- Typical host: rack-mounted Computronics light board

## Usage

```lua
local component = require("component")
local board = component.light_board

if not board then
  error("light_board component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Board Layout

The number of addressable lights depends on the physical board mode:

- `4` lights in the default mode
- `5` lights in the regular mode
- `10` lights in the five-by-two style mode
- `12` lights in the narrow twelve-light mode
- `42` lights in the full matrix mode

The mode is changed physically by activating the mounted board with a suitable tool. Lua code can read the current light count, but it cannot switch the layout.

Light indexes in Lua always start at `1`.

## API

### light_count

- Syntax: `board.light_count()`
- Returns: `number`
- Purpose: Returns how many light slots are available in the current board layout.

Example:

```lua
print("Light slots:", board.light_count())
```

### setColor

- Syntax: `board.setColor(index, color)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Sets the stored RGB color of one light.

Parameters:

- `index:number`: light index, starting at `1`.
- `color:number`: 24-bit RGB color in `0xRRGGBB` form.

Behavior:

- Valid color values are `0` through `16777215` (`0xFFFFFF`).
- Changing a color consumes component energy.
- The board stores the color even if that light is currently off.

Common failure results:

- `false, "not enough energy"`
- `false, "number must be between 0 and 16777215"`

Errors:

- Invalid light indexes raise `index out of range`.

Example:

```lua
local ok, reason = board.setColor(1, 0x33CCFF)
if not ok then
  io.stderr:write("setColor failed: " .. tostring(reason) .. "\n")
end
```

### getColor

- Syntax: `board.getColor(index)`
- Returns: `number`
- Purpose: Returns the stored color of a light.

Parameters:

- `index:number`: light index, starting at `1`.

This returns the configured color whether or not the light is currently active.

Errors:

- Invalid light indexes raise `index out of range`.

Example:

```lua
print(string.format("Slot 1 color: 0x%06X", board.getColor(1)))
```

### setActive

- Syntax: `board.setActive(index, active)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Turns a light on or off.

Parameters:

- `index:number`: light index, starting at `1`.
- `active:boolean`: `true` to enable the light, `false` to disable it.

Behavior:

- Changing the state consumes component energy.
- While a light is active, the board consumes maintenance energy every tick.
- If the rack can no longer pay that maintenance cost, active lights are turned off automatically.

Common failure results:

- `false, "not enough energy"`

Errors:

- Invalid light indexes raise `index out of range`.

Example:

```lua
assert(board.setColor(1, 0x00FF00))
assert(board.setActive(1, true))
```

### isActive

- Syntax: `board.isActive(index)`
- Returns: `boolean`
- Purpose: Returns whether a light is currently on.

Parameters:

- `index:number`: light index, starting at `1`.

Errors:

- Invalid light indexes raise `index out of range`.

Example:

```lua
if board.isActive(1) then
  print("Light 1 is on")
end
```

## Example

The following script paints the whole board red, then turns every light on.

```lua
local count = board.light_count()

for i = 1, count do
  assert(board.setColor(i, 0xFF3333))
  assert(board.setActive(i, true))
end
```

## Notes

- Color changes and state changes both use the component network energy buffer.
- Keeping many lights enabled continuously requires a steady power supply because active lights consume maintenance energy.

## Related

- `component`
- `rack_capacitor`
- `switch_board`
