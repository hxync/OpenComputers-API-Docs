# opencomputers-wake-message-aware

## Summary

This page documents the wake-message callbacks shared by modem-like components that can start a stopped computer when a matching network packet arrives.

## Availability

- Repository: `opencomputers`
- Typical Lua component names: exposed by wake-capable networking components such as modems
- Backing trait: `WakeMessageAware`

## What It Does

- Stores an optional wake-up message string.
- Optionally enables fuzzy matching so extra packet parameters are ignored.
- Starts the host computer when an incoming packet matches the configured wake message.

## Matching Rules

- Exact mode: the packet data must contain exactly one message item and that item must match the configured wake message.
- Fuzzy mode: only the first message item must match; additional packet arguments are ignored.
- Matching accepts either Lua strings or raw byte arrays that decode to the configured UTF-8 message.

Wake-up matching ignores the packet port because ports are closed while the computer is stopped.

## API

### getWakeMessage

- Syntax: `component.getWakeMessage()`
- Returns: `string or nil, boolean`
- Purpose: Returns the currently configured wake message and the fuzzy-match flag.

### setWakeMessage

- Syntax: `component.setWakeMessage(message[, fuzzy])`
- Returns: `oldMessage or nil, oldFuzzy`
- Purpose: Changes the wake message and returns the previous configuration.

Behavior:

- Passing `nil` as the first argument clears the wake message entirely.
- If `fuzzy` is omitted, the previous fuzzy setting is kept.

## Example

```lua
local component = require("component")
local modem = component.modem

local oldMessage, oldFuzzy = modem.setWakeMessage("wake", true)
print("Previous wake config:", oldMessage, oldFuzzy)

local message, fuzzy = modem.getWakeMessage()
print("Current wake config:", message, fuzzy)
```

## Related

- `modem`
- `opencomputers-redstone-signaller`
