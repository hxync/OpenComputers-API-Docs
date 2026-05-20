# stargatetech2

## Summary

This page documents the StargateTech 2 abstract bus integration exposed to Lua by OpenComputers.

## Availability

- Dependency: `stargatetech2`
- Label: `integration-required`

## Component Names

- `abstract_bus`

## What This Integration Is Good For

- Enabling or disabling a local abstract bus interface.
- Reading and changing the local bus address.
- Scanning the bus for other devices.
- Sending small structured packets over the abstract bus.

## Components

### abstract_bus

`abstract_bus` is exposed by the StargateTech 2 abstract bus card.

#### `getEnabled()`

- Syntax: `getEnabled(): boolean`
- Purpose: Return whether the local bus interface is enabled.
- Returns:
  - `true` if the interface is enabled.
  - `false` otherwise.

#### `setEnabled(enabled)`

- Syntax: `setEnabled(enabled: boolean): boolean`
- Purpose: Enable or disable the local bus interface.
- Parameters:
  - `enabled: boolean`
    Desired enabled state.
- Returns:
  - The new enabled state.

#### `getAddress()`

- Syntax: `getAddress(): number`
- Purpose: Return the local interface address.
- Returns:
  - Current 16-bit bus address.

#### `setAddress(address)`

- Syntax: `setAddress(address: number): number`
- Purpose: Set the local interface address.
- Parameters:
  - `address: number`
    New address. Only the lower 16 bits are kept.
- Returns:
  - The stored 16-bit address.

#### `send(address, data)`

- Syntax: `send(address: number, data: table): table[, string]`
- Purpose: Send one structured packet across the abstract bus.
- Parameters:
  - `address: number`
    Target bus address.
  - `data: table`
    Key-value table to serialize into a LIP packet.
- Returns:
  - A table of responses on success.
  - `nil, "not enough energy"` if the local node lacks energy.
- Usage notes:
  - Keys and values are converted to strings before transmission.
  - Total packet size is limited by the OpenComputers `maxNetworkPacketSize` setting.
  - Oversized packets raise an argument error such as `packet too big`.

#### `scan(mask)`

- Syntax: `scan(mask: number): table[, string]`
- Purpose: Scan the abstract bus for visible devices.
- Parameters:
  - `mask: number`
    16-bit scan mask.
- Returns:
  - A table of discovered devices on success.
  - `nil, "not enough energy"` if the local node lacks energy.

#### `maxPacketSize()`

- Syntax: `maxPacketSize(): number`
- Purpose: Return the maximum packet size allowed by the current OpenComputers configuration.
- Returns:
  - Maximum packet size in characters.

#### Event: `bus_message`

- Syntax: `bus_message(protocolId, sender, target, data, metadata)`
- Purpose: Notify the owning machine when a compatible abstract bus packet arrives.
- Parameters:
  - `protocolId: number`
    Bus protocol id of the received packet.
  - `sender: number`
    Source bus address.
  - `target: number`
    Target bus address.
  - `data: table`
    Received key-value payload.
  - `metadata: table`
    Metadata table containing fields such as `mod`, `device`, and `player`.

## Example

```lua
local component = require("component")
local bus = component.abstract_bus

print("Address:", bus.getAddress())
print("Enabled:", bus.getEnabled())

local devices = bus.scan(0xFFFF)
print("Found devices:", #devices)
```

## Related

- `gc`
- `network`
