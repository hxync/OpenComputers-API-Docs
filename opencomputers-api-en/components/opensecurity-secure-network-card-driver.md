# modem

## Summary

This page documents the extra callback added by OpenSecurity's secure network card driver. The generated filename is `opensecurity-secure-network-card-driver.md`, but the actual component name exposed by the item is still `modem`.

Because the driver subclasses OpenComputers' standard network card implementation, the normal modem/network-card API remains available. OpenSecurity adds one extra callback: `generateUUID()`.

## Availability

- Repository: `opensecurity`
- Actual component name: `modem`
- Typical host: OpenSecurity secure network card

## Usage

```lua
local component = require("component")
local modem = component.modem
```

If multiple modem-like devices are installed, prefer `component.proxy(address)` to target the secure card explicitly.

## Extra API

### generateUUID

- Syntax: `modem.generateUUID()`
- Returns: `true`
- Purpose: Rebuilds the modem node so it gets a new network address.

Behavior:

- The current node address is discarded.
- A fresh modem node with a new UUID/address is created.
- The driver stores the old neighbor list, joins a new OC network, then reconnects the saved neighbors.
- Existing code that cached the old component address must rediscover the device after this call.

Example:

```lua
print("Old address:", modem.address)
assert(modem.generateUUID())
```

## Notes

- This callback does not expose a failure return in the current source.
- The item still behaves as a normal modem aside from the address randomization callback.

## Related

- `component`
- `opencomputers-network-card`
