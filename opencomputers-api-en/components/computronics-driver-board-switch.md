# switch_board

## Summary

The `switch_board` component is a rack-mounted four-switch input board from Computronics. It can be toggled from Lua or by physically clicking the board in the rack, making it useful for simple control panels and machine-room toggles.

Each switch is individually addressable and can emit a `computer.signal` event when its state changes.

## Availability

- Repository: `computronics`
- Component name: `switch_board`
- Typical host: rack-mounted Computronics switch board

## Usage

```lua
local component = require("component")
local switches = component.switch_board

if not switches then
  error("switch_board component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Switch Layout

- The board exposes exactly `4` switches.
- Lua indexes start at `1`.
- Each active switch consumes maintenance energy every tick.
- If the rack runs out of power for maintenance, active switches are turned off automatically.

Whenever a switch changes state through user interaction or through `setActive`, the board emits:

```lua
"switch_flipped", index, state
```

inside a `computer.signal` event.

## API

### setActive

- Syntax: `switches.setActive(index, active)`
- Returns: `boolean`
- Purpose: Sets one switch to the requested state.

Parameters:

- `index:number`: switch index from `1` to `4`.
- `active:boolean`: desired state.

Return behavior:

- Returns `true` if the state actually changed.
- Returns `false` if the switch was already in the requested state.

Errors:

- Invalid indexes raise `index out of range: N`.

Example:

```lua
local changed = switches.setActive(2, true)
print("State changed:", changed)
```

### isActive

- Syntax: `switches.isActive(index)`
- Returns: `boolean`
- Purpose: Returns whether the specified switch is currently on.

Parameters:

- `index:number`: switch index from `1` to `4`.

Errors:

- Invalid indexes raise `index out of range: N`.

Example:

```lua
for i = 1, 4 do
  print(i, switches.isActive(i))
end
```

## Example

Wait for physical or scripted switch changes:

```lua
local event = require("event")

while true do
  local _, _, _, name, index, state = event.pull("computer.signal")
  if name == "switch_flipped" then
    print("Switch", index, "is now", state and "on" or "off")
  end
end
```

## Notes

- The board has no callback for retrieving the number of switches because it is always fixed at four.
- Programmatic changes and physical front-panel toggles both use the same `switch_flipped` event.

## Related

- `component`
- `light_board`
- `rack_capacitor`
