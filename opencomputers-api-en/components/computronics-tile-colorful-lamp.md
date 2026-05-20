# colorful_lamp

## Summary

The `colorful_lamp` component controls a Computronics colored lamp block. It lets Lua read or change the lamp's stored color value, which can be used for status indicators, machine states, or bundled-color displays.

This lamp stores a 15-bit color-style value rather than a 24-bit `0xRRGGBB` color like the rack light board.

## Availability

- Repository: `computronics`
- Component name: `colorful_lamp`
- Typical host: placed Computronics colorful lamp block

## Usage

```lua
local component = require("component")
local lamp = component.colorful_lamp

if not lamp then
  error("colorful_lamp component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Color Model

- `0` turns the lamp off.
- Non-zero values turn the lamp on.
- The stored color is masked to `15` bits internally.

The lamp is also designed to react to bundled wiring integrations. When bundled inputs are used, each of the lower 15 bits corresponds to one bundled color line.

## API

### getLampColor

- Syntax: `lamp.getLampColor()`
- Returns: `number`
- Purpose: Returns the lamp's current stored color value.

Example:

```lua
print("Lamp color value:", lamp.getLampColor())
```

### setLampColor

- Syntax: `lamp.setLampColor(color)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Changes the lamp color value.

Parameters:

- `color:number`: lamp color value.

Behavior:

- `0` turns the lamp off.
- The implementation accepts non-negative integers and stores only the lower 15 bits.
- Although the wrapper checks for values up to `65535`, the returned error text says the intended range is `0` to `32767`.
- For predictable scripts, stay within `0` to `32767`.

Common failure results:

- `false, "number must be between 0 and 32767"`

Example:

```lua
assert(lamp.setLampColor(0x1234))
```

Example: turn the lamp off.

```lua
assert(lamp.setLampColor(0))
```

## Notes

- If the colored-light feature is enabled, the block uses the stored color value directly for light output.
- If colored lighting is disabled in the environment, any non-zero value still lights the lamp at normal brightness.

## Related

- `component`
- `colors`
