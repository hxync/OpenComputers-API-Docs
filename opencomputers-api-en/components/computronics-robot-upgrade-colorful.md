# colors

## Summary

The `colors` component is the Computronics robot color overlay upgrade. It lets Lua apply a colored overlay to the robot model, which is useful for team colors, machine state display, or role identification.

The upgrade stores either a 24-bit color or a special reset value that returns the robot to its normal appearance.

## Availability

- Repository: `computronics`
- Component name: `colors`
- Typical host: robot with the Computronics colorful upgrade installed

## Usage

```lua
local component = require("component")
local colors = component.colors

if not colors then
  error("colors component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### setColor

- Syntax: `colors.setColor(color)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Applies a new overlay color to the robot.

Parameters:

- `color:number`: 24-bit RGB color in `0xRRGGBB` form.

Behavior:

- Valid values are `0` through `16777215` (`0xFFFFFF`).
- Changing the robot color consumes component energy.

Common failure results:

- `false, "not enough energy"`
- `false, "number must be between 0 and 16777215"`

Example:

```lua
assert(colors.setColor(0xFF8800))
```

### getColor

- Syntax: `colors.getColor()`
- Returns: `number`
- Purpose: Returns the currently stored robot overlay color.

Return details:

- A normal color is returned as a 24-bit integer.
- `-1` means the robot is using its default appearance with no custom overlay.

Example:

```lua
local color = colors.getColor()
if color == -1 then
  print("Robot uses default colors")
else
  print(string.format("Robot color: 0x%06X", color))
end
```

### resetColor

- Syntax: `colors.resetColor()`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Clears the custom overlay and restores the robot's default appearance.

Behavior:

- Internally this sets the stored color to `-1`.
- Resetting the color consumes the same energy as assigning a new color.

Common failure results:

- `false, "not enough energy"`

Example:

```lua
assert(colors.resetColor())
```

## Example

```lua
local level = 0.85

if level > 0.75 then
  colors.setColor(0x00CC00)
elseif level > 0.35 then
  colors.setColor(0xFFCC00)
else
  colors.setColor(0xCC0000)
end
```

## Related

- `component`
- `colorful_lamp`
