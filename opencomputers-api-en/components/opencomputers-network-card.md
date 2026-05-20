# modem

## Summary

This page documents the wired `modem` component exposed by the standard OpenComputers network card. It lets a machine open ports, check port state, send packets to a specific address, and broadcast packets across the reachable wired network.

In generated docs this often appears as `opencomputers-network-card`, but the actual component name visible to Lua is `modem`.

## Availability

- Repository: `opencomputers`
- Lua component name: `modem`
- Typical host: wired network card, rack network interface, devices exposing the wired modem implementation

## Important Notes

- Valid port numbers are `1..65535`.
- Opening too many ports raises `"too many open ports"`.
- Ports are cleared when the attached computer starts or stops.
- This wired card reports `isWireless() == false` and `isWired() == true`.

## Usage

```lua
local component = require("component")
local modem = component.modem

if not modem then
  error("modem component not available")
end
```

## Receiving Packets

Incoming packets arrive as `modem_message` signals when the destination port is open on this card.

Typical event payload:

```lua
"modem_message", receiverAddress, senderAddress, port, distance, ...
```

On a purely wired modem, distance is typically `0`.

## API

### open

- Syntax: `modem.open(port)`
- Returns: `boolean`
- Purpose: Opens one local port for incoming packets.

Parameter:

- `port:number`: port number in the range `1..65535`.

Return value details:

- `true` when the port was newly opened,
- `false` when it was already open.

Possible error from source: `"too many open ports"`.

### close

- Syntax:
  - `modem.close()`
  - `modem.close(port)`
- Returns: `boolean`
- Purpose: Closes one port, or all open ports when called without arguments.

Return value details:

- without an argument: `true` if any ports were open and got cleared,
- with a port: `true` if that port was open and got closed.

### isOpen

- Syntax: `modem.isOpen(port)`
- Returns: `boolean`
- Purpose: Checks whether a local port is currently open.

### isWireless

- Syntax: `modem.isWireless()`
- Returns: `boolean`
- Purpose: Reports whether this modem supports wireless transmission.

For the wired card described on this page, this returns `false`.

### isWired

- Syntax: `modem.isWired()`
- Returns: `boolean`
- Purpose: Reports whether this modem supports wired network traffic.

For the wired card described on this page, this returns `true`.

### send

- Syntax: `modem.send(address, port, data...)`
- Returns: `boolean`
- Purpose: Sends a packet to a specific target node address.

Parameters:

- `address:string`: destination node address.
- `port:number`: destination port in the range `1..65535`.
- `data...`: payload values.

Behavior:

- The packet is transmitted across the reachable wired network or rack-neighbor network, depending on host type.
- The call returns `true` after queueing the send operation.

### broadcast

- Syntax: `modem.broadcast(port, data...)`
- Returns: `boolean`
- Purpose: Sends a packet to every reachable receiver listening on the specified port.

Parameters:

- `port:number`: destination port in the range `1..65535`.
- `data...`: payload values.

This is the usual way to implement discovery protocols or simple LAN chat.

### maxPacketSize

- Syntax: `modem.maxPacketSize()`
- Returns: `number`
- Purpose: Gets the configured maximum packet size.

This is the hard upper bound for one packet's payload encoding.

## Example

```lua
local event = require("event")

modem.open(1234)
modem.broadcast(1234, "hello", os.time())

local _, _, from, port, _, message = event.pull("modem_message")
print("From:", from, "Port:", port, "Message:", message)
```

## Related

- `wireless modem`
- `tunnel`
- `relay`
- `component`
