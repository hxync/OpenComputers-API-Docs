# os_magreader

## Summary

The `os_magreader` component is OpenSecurity's magnetic card reader. The Lua API itself is small, but the block emits a configurable signal whenever a programmed magnetic card is swiped through it.

## Availability

- Repository: `opensecurity`
- Component name: `os_magreader`
- Typical host: placed OpenSecurity magnetic card reader

## Usage

```lua
local component = require("component")
local mag = component.os_magreader

if not mag then
  error("os_magreader component not installed")
end
```

## Swipe Behavior

When a valid magnetic card is swiped and the reader has enough power, it sends:

```lua
computer.signal(eventName, user, data, uuid, locked, side)
```

Default event name:

- `magData`

Payload fields:

- `user`:
  - player display name when `magCardDisplayName` is enabled
  - literal `"player"` when that config is disabled
- `data`: the card payload string
- `uuid`:
  - real card UUID normally
  - `"-1"` when `ignoreUUIDs` is enabled
- `locked:boolean`: whether the card is rewrite-locked
- `side:number`: the side from which the swipe interaction hit the block

Additional behavior:

- Swipes consume `5` buffer energy before the signal is emitted.
- The block plays the `opensecurity:card_swipe` sound when a magnetic card is used while idle.
- If the card has no stored `data` tag, no signal is emitted.

## API

### greet

- Syntax: `mag.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

### setEventName

- Syntax: `mag.setEventName(name)`
- Returns: `boolean`
- Purpose: Changes the signal event name used for future swipes.

Parameters:

- `name:string`: new signal name.

Behavior:

- The name is stored in NBT.
- After world reload, empty or missing values fall back to `magData`.

Example:

```lua
assert(mag.setEventName("doorCard"))
```

## Example

```lua
local event = require("event")

mag.setEventName("doorCard")

while true do
  local _, _, _, eventName, user, data, uuid, locked, side = event.pull("computer.signal")
  if eventName == "doorCard" then
    print(user, data, uuid, locked, side)
  end
end
```

## Related

- `component`
- `os_cardwriter`
