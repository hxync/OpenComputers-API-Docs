# tunnel

## Summary

This page documents the `tunnel` component provided by linked cards. A linked card is a point-to-point or shared-channel long-distance communication device that bypasses normal local network range limits.

Generated docs often call this `opencomputers-linked-card`, but the actual Lua component name is `tunnel`.

## Availability

- Repository: `opencomputers`
- Lua component name: `tunnel`
- Typical hosts: linked cards

## Channel Model

Every linked card belongs to a shared channel string. In source, the stored channel field is called `tunnel`.

Cards on the same channel can exchange packets with one another:

- packets are delivered to every other endpoint on that channel,
- the sending endpoint does not receive its own packet.

## Usage

```lua
local component = require("component")
local tunnel = component.tunnel

if not tunnel then
  error("tunnel component not available")
end
```

## Receiving Messages

Incoming linked-card traffic is delivered as `modem_message`-style packet reception through the component networking layer. In practice, linked cards are commonly handled using event listeners on the host machine.

## API

### send

- Syntax: `tunnel.send(data...)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Sends one packet to every other linked card on the same channel.

Parameters:

- `data...`: packet payload.

Behavior:

- The sender does not receive its own packet.
- Delivery range is not limited like normal wireless modem range.
- Sending consumes a comparatively large amount of energy.

Failure reason from source: `"not enough energy"`.

### maxPacketSize

- Syntax: `tunnel.maxPacketSize()`
- Returns: `number`
- Purpose: Gets the configured maximum packet size.

### getChannel

- Syntax: `tunnel.getChannel()`
- Returns: `string`
- Purpose: Reads the channel identifier shared by this linked card.

Example:

```lua
print("Linked channel:", tunnel.getChannel())
tunnel.send("sync", os.time())
```

## Related

- `modem`
- `wireless modem`
- `component`
