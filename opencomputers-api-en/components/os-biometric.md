# os_biometric

## Summary

The `os_biometric` component is OpenSecurity's biometric reader. It scans a player who uses the block and then sends a signal event containing that player's UUID or a Base64-encoded copy of that UUID, depending on the OpenSecurity `returnRealUUID` setting.

It is useful for access control, identity logging, security doors, and authentication workflows.

## Availability

- Repository: `opensecurity`
- Component name: `os_biometric`
- Typical host: placed OpenSecurity biometric reader

## Usage

```lua
local component = require("component")
local bio = component.os_biometric

if not bio then
  error("os_biometric component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Event Behavior

When a player is scanned by the block, the component sends:

```lua
computer.signal(eventName, payload)
```

Default event name:

- `bioReader`

Payload behavior:

- if `OpenSecurity.returnRealUUID` is enabled, the payload is the player's real UUID string
- otherwise the payload is the Base64 encoding of the UUID string

## API

### greet

- Syntax: `bio.greet()`
- Returns: `string`
- Purpose: Returns a built-in greeting string from the device.

In this version, the returned text is:

```text
Lasciate ogne speranza, voi ch'intrate
```

Example:

```lua
print(bio.greet())
```

### setEventName

- Syntax: `bio.setEventName(name)`
- Returns: `boolean`
- Purpose: Sets the signal event name used when a player is scanned.

Parameters:

- `name:string`: new event name.

Behavior:

- The name is persisted to NBT.
- Empty names are not preserved across reload; when reloaded without a valid stored name, the device falls back to `bioReader`.

Example:

```lua
assert(bio.setEventName("doorAuth"))
```

## Example

```lua
local event = require("event")

bio.setEventName("doorAuth")

while true do
  local _, _, _, eventName, payload = event.pull("computer.signal")
  if eventName == "doorAuth" then
    print("Scanned identity:", payload)
  end
end
```

## Related

- `component`
- `os_cardwriter`
