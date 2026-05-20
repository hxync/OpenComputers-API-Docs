# os_keypad

## Summary

The `os_keypad` component controls an OpenSecurity keypad lock. It can rename the emitted keypress event, toggle keypad beep sounds, customize the display text, and rewrite the labels and colors of the 12 keypad buttons.

## Availability

- Repository: `opensecurity`
- Component name: `os_keypad`
- Typical host: placed OpenSecurity keypad lock

## Usage

```lua
local component = require("component")
local keypad = component.os_keypad

if not keypad then
  error("os_keypad component not installed")
end
```

## Keypress Signals

When a player presses a key, the keypad sends:

```lua
computer.signal(eventName, buttonIndex, label)
```

Default event name:

- `keypad`

Payload fields:

- `buttonIndex:number`: `1` through `12`
- `label:string`: the current label shown on that button

Default button labels:

- `1 2 3`
- `4 5 6`
- `7 8 9`
- `* 0 #`

## Text And Color Limits

- Display text is trimmed to `8` characters.
- Button labels are trimmed to `3` characters.
- Color values are masked with `& 7`, so only the low three bits are used.
- Valid effective colors are therefore `0` through `7`.

## API

### setEventName

- Syntax: `keypad.setEventName(name)`
- Returns: `boolean`
- Purpose: Changes the event name sent on key presses.

Parameters:

- `name:string`: new event name.

Example:

```lua
assert(keypad.setEventName("doorPad"))
```

### setShouldBeep

- Syntax: `keypad.setShouldBeep(enabled)`
- Returns: `boolean`
- Purpose: Enables or disables the keypad click sound when buttons are pressed.

Parameters:

- `enabled:boolean`: `true` to beep, `false` to stay silent.

Behavior:

- The default state is `true`.
- The setting is persisted in NBT.

Example:

```lua
assert(keypad.setShouldBeep(false))
```

### setDisplay

- Syntax: `keypad.setDisplay(text[, color])`
- Returns: `boolean`
- Purpose: Updates the keypad display text and optional display color.

Parameters:

- `text:string`: text shown on the display. Longer strings are truncated to `8` characters.
- `color:number` optional: display color value masked to `0` through `7`.

Example:

```lua
assert(keypad.setDisplay("OPEN", 2))
```

### setKey

- Syntax:
  - `keypad.setKey(index, text[, color])`
  - `keypad.setKey(labelTable[, colorTable])`
- Returns: `boolean`
- Purpose: Replaces one key label or rewrites the full keypad layout.

Parameters for single-key mode:

- `index:number`: key number from `1` through `12`
- `text:string`: new label, truncated to `3` characters
- `color:number` optional: key color masked to `0` through `7`

Parameters for table mode:

- `labelTable:table`: table keyed by button numbers `1` through `12`
- `colorTable:table` optional: table keyed by button numbers `1` through `12`

Behavior:

- Passing an out-of-range index raises `Index <n> is out of range`.
- Passing the wrong first argument type raises `First argument must be index or table`.

Example:

```lua
assert(keypad.setKey(10, "CLR", 4))
assert(keypad.setKey({
  [1] = "A",
  [2] = "B",
  [3] = "C"
}, {
  [1] = 1,
  [2] = 2,
  [3] = 4
}))
```

## Example

```lua
local event = require("event")

keypad.setEventName("doorPad")
keypad.setDisplay("LOCKED", 4)
keypad.setShouldBeep(true)

while true do
  local _, _, _, eventName, index, label = event.pull("computer.signal")
  if eventName == "doorPad" then
    print(index, label)
  end
end
```

## Related

- `component`
- `os_door`
