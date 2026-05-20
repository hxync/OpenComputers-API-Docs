# rack_capacitor

## Summary

The `rack_capacitor` component is a rack-mounted energy buffer from Computronics. It exposes the stored charge of the mountable card itself, which is useful when you want to monitor rack power headroom from Lua.

This is a read-only power component. It does not provide callbacks for charging, discharging, or switching modes.

## Availability

- Repository: `computronics`
- Component name: `rack_capacitor`
- Typical host: rack-mounted Computronics capacitor board

## Usage

```lua
local component = require("component")
local capacitor = component.rack_capacitor

if not capacitor then
  error("rack_capacitor component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### energy

- Syntax: `capacitor.energy()`
- Returns: `number`
- Purpose: Returns the amount of energy currently stored in this capacitor's local OpenComputers buffer.

The value is reported in the internal energy unit used by the component network buffer.

Example:

```lua
print("Stored energy:", capacitor.energy())
```

### maxEnergy

- Syntax: `capacitor.maxEnergy()`
- Returns: `number`
- Purpose: Returns the maximum amount of energy the capacitor can store.

The default configured capacity is `7500`, but modpack or server configuration may change that value.

Example:

```lua
local current = capacitor.energy()
local max = capacitor.maxEnergy()
print(string.format("Capacitor charge: %.0f / %.0f", current, max))
```

## Example

```lua
local current = capacitor.energy()
local max = capacitor.maxEnergy()

if current < max * 0.25 then
  print("Rack capacitor is running low")
end
```

## Related

- `component`
- `light_board`
- `switch_board`
