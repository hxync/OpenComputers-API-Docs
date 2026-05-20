# modem

## Summary

This Computronics spoofing card exposes itself as a `modem` component, but it allows Lua to forge the packet source address for `send` and `broadcast`.

It is useful for protocol testing, packet simulation, spoofed-device emulation, and debugging software that expects traffic from specific addresses.

## Availability

- Repository: `computronics`
- Component name: `modem`
- Typical host: device equipped with the Computronics spoofing card

## Usage

```lua
local component = require("component")
local modem = component.modem

if not modem then
  error("modem component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Important Behavior

- This component inherits the normal OpenComputers wired modem API.
- In addition to the standard modem callbacks, its `send` and `broadcast` methods accept an optional spoofed source address.
- Every spoofed packet transmission consumes additional spoofing-card energy.
- Port numbers must be between `1` and `65535`.

Because it extends the normal network card implementation, the component also supports the usual modem callbacks such as:

- `open`
- `close`
- `isOpen`
- `isWireless`
- `isWired`
- `maxPacketSize`

Those inherited callbacks behave like a standard wired modem.

## API

### send

- Syntax: `modem.send(targetAddress[, sourceAddress], port, data...)`
- Returns:
  - Success: `true`
  - Failure: `false`
- Purpose: Sends a packet to a specific target, optionally pretending that the packet originated from another source address.

Parameters:

- `targetAddress:string`: destination component address.
- `sourceAddress:string` optional: fake source address to embed in the packet.
- `port:number`: target port from `1` to `65535`.
- `data...`: payload values.

Behavior:

- If `sourceAddress` is omitted, the real component address is used.
- If the component lacks enough spoofing energy, the method returns `false`.
- Invalid ports raise `invalid port number`.

Example: normal send using the real source address.

```lua
assert(modem.send("12345678-1234-1234-1234-123456789abc", 42, "ping"))
```

Example: spoofed source address.

```lua
assert(modem.send(
  "12345678-1234-1234-1234-123456789abc",
  "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  42,
  "spoofed payload"
))
```

### broadcast

- Syntax: `modem.broadcast([sourceAddress], port, data...)`
- Returns:
  - Success: `true`
  - Failure: `false`
- Purpose: Broadcasts a packet on a port, optionally using a forged source address.

Parameters:

- `sourceAddress:string` optional: fake source address to embed in the packet.
- `port:number`: broadcast port from `1` to `65535`.
- `data...`: payload values.

Behavior:

- If `sourceAddress` is omitted, the real component address is used.
- If the component lacks enough spoofing energy, the method returns `false`.
- Invalid ports raise `invalid port number`.

Example:

```lua
assert(modem.broadcast(100, "status", true))
```

Example with spoofed sender:

```lua
assert(modem.broadcast(
  "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  100,
  "simulated-node-online"
))
```

## Notes

- This page documents the spoofing extensions. For inherited port-management behavior, treat the component like a standard wired OpenComputers modem.
- Receiving packets still depends on opened ports in the inherited modem implementation.

## Related

- `component`
- `opencomputers-network-card`
