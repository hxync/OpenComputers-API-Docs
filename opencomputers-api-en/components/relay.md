# relay

## Summary

The `relay` component is the programmable surface of the relay block. It bridges subnetworks while keeping ordinary components local, and it can be extended with wireless cards or linked cards for long-distance packet forwarding.

## Availability

- Repository: `opencomputers`
- Lua component name: `relay`
- Typical host: relay block

## What It Does

- Bridges packets between wired subnetwork segments without exposing all components across the whole network.
- Optionally forwards packets wirelessly when a wireless network card is installed.
- Optionally forwards packets through a linked-card tunnel when a linked card is installed.
- Supports repeater mode for re-broadcasting received wireless packets.

## Hardware-Dependent Behavior

- Without a wireless card, wireless range is effectively disabled.
- With a tier 1 or tier 2 wireless card, the relay gains a configurable wireless forwarding range.
- With a linked card, the relay can forward through the linked-card tunnel identified by that card's stored tunnel id.

The relay can also be upgraded with CPU, memory, and storage parts to improve forwarding delay, throughput, and queue size.

## API

### getStrength

- Syntax: `relay.getStrength()`
- Returns: `number`
- Purpose: Returns the wireless relay range currently configured for packet forwarding.

### setStrength

- Syntax: `relay.setStrength(strength)`
- Parameters:
  - `strength:number`: desired wireless forwarding range.
- Returns: `number`
- Purpose: Sets the wireless forwarding range and returns the applied value.

Behavior:

- The value is clamped to `0..maxWirelessRange` for the installed wireless card tier.
- If no wireless card is installed, the effective maximum is `0`.

### isRepeater

- Syntax: `relay.isRepeater()`
- Returns: `boolean`
- Purpose: Returns whether incoming wireless packets may be broadcast back out wirelessly.

### setRepeater

- Syntax: `relay.setRepeater(enabled)`
- Parameters:
  - `enabled:boolean`: desired repeater state.
- Returns: `boolean`
- Purpose: Enables or disables repeater mode and returns the new state.

## Practical Notes

- Relay loops can still duplicate packets; the block does not keep a recent-forward history.
- Wireless forwarding only happens when range is greater than `0` and enough energy is available.
- Linked-card forwarding only happens for packets that entered from a specific side, not for every internal relay path.
- ComputerCraft integration is also present in the implementation and mirrors modem-style events for attached ComputerCraft computers.

## Example

```lua
local component = require("component")
local relay = component.relay

print("Current strength:", relay.getStrength())
print("Repeater mode:", relay.isRepeater())

relay.setStrength(16)
relay.setRepeater(true)

print("Applied strength:", relay.getStrength())
print("Repeater mode now:", relay.isRepeater())
```

## Related

- `access_point`
- `modem`
- `tunnel`
