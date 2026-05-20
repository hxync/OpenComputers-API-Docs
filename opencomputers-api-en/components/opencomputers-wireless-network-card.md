# wireless modem

## Summary

This page documents the wireless `modem` component provided by OpenComputers wireless network cards. It extends the normal modem API with configurable signal strength, allowing packets to travel wirelessly across distance and even across dimensions when the wireless network permits it.

In generated docs this often appears as `opencomputers-wireless-network-card`, but the actual Lua component name is still `modem`.

## Availability

- Repository: `opencomputers`
- Lua component name: `modem`
- Typical hosts: wireless network cards

## Tier Behavior

- Tier 1 wireless cards send and receive wireless packets only.
- Tier 2 wireless cards support wireless traffic and also participate in wired traffic.

This difference is reflected by:

- `isWireless() == true` on both tiers,
- `isWired() == false` on tier 1,
- `isWired() == true` on tier 2.

## Wireless Strength

The strength value is the maximum transmission range in blocks. Higher strength means:

- longer potential range,
- higher energy cost per sent wireless packet.

Source clamps strength into `0 .. maxWirelessRangeForThisTier`.

## Usage

```lua
local component = require("component")
local modem = component.modem

if not modem or not modem.isWireless() then
  error("wireless modem not available")
end
```

## API

### getStrength

- Syntax: `modem.getStrength()`
- Returns: `number`
- Purpose: Reads the current wireless transmission range.

### setStrength

- Syntax: `modem.setStrength(strength)`
- Returns: `number`
- Purpose: Changes wireless transmission range and returns the applied value.

Parameter:

- `strength:number`: desired transmission range.

Behavior:

- Values below `0` become `0`.
- Values above the tier maximum are clamped down.

Example:

```lua
local applied = modem.setStrength(32)
print("Wireless range:", applied)
```

## Inherited Modem API

Wireless cards also support the standard modem API:

- `open`
- `close`
- `isOpen`
- `send`
- `broadcast`
- `maxPacketSize`
- `isWireless`
- `isWired`

See the wired `modem` page for port rules and `modem_message` event structure.

## Error Behavior

When sending wireless traffic with strength above zero, the card consumes energy proportional to the configured range. If there is not enough energy, send and broadcast operations can fail with:

- `"not enough energy"`

## Example

```lua
modem.open(42)
modem.setStrength(16)
modem.broadcast(42, "ping")
```

## Related

- `modem`
- `tunnel`
- `access_point`
- `component`
