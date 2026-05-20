# access_point

## Summary

The `access_point` component is the programmable surface of the deprecated access point block. It behaves like a wireless-capable switch and can relay packets between wired subnetworks and the wireless network.

It can also act as a wireless repeater, re-broadcasting received wireless packets back into the wireless medium.

## Availability

- Repository: `opencomputers`
- Lua component name: `access_point`
- Typical host: access point block

## Important Notes

- The access point block is deprecated upstream in favor of relays.
- Wireless relay range is controlled by strength.
- Repeater mode decides whether received wireless packets are also retransmitted wirelessly.
- Like switches, access points can create duplicate packets if you build loops.

## Usage

```lua
local component = require("component")
local ap = component.access_point

if not ap then
  error("access_point component not available")
end
```

## API

### getStrength

- Syntax: `ap.getStrength()`
- Returns: `number`
- Purpose: Reads the wireless relay range used when the access point retransmits packets.

### setStrength

- Syntax: `ap.setStrength(strength)`
- Returns: `number`
- Purpose: Changes wireless relay range and returns the applied value.

Parameter:

- `strength:number`: desired relay range.

The implementation stores and persists this value, but this build uses unusual clamping behavior. In practice, keep it within the normal tier-two wireless range.

### isRepeater

- Syntax: `ap.isRepeater()`
- Returns: `boolean`
- Purpose: Checks whether the access point is currently repeating received wireless packets back onto the wireless network.

When disabled, the access point can still bridge wired and wireless traffic in other directions handled by the switch logic; this flag specifically controls wireless re-broadcast behavior.

### setRepeater

- Syntax: `ap.setRepeater(enabled)`
- Returns: `boolean`
- Purpose: Enables or disables repeater mode and returns the new state.

Parameter:

- `enabled:boolean`: desired repeater state.

## Relay Behavior

At runtime, the access point:

- relays packets like a switch across connected sides,
- can additionally forward packets into the wireless network,
- consumes energy for wireless retransmission based on current strength.

It does not keep a full memory of recently forwarded packets, so cyclic topologies can produce repeated deliveries.

## Example

```lua
print("Repeater:", ap.isRepeater())
ap.setRepeater(true)
ap.setStrength(24)
print("Range:", ap.getStrength())
```

## Related

- `modem`
- `wireless modem`
- `relay`
- `component`
