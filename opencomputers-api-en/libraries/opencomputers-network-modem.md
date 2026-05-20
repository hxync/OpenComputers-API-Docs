# modem

## Summary

Software Ethernet-style driver built on OpenComputers modem components and a simple discovery protocol.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\modem.lua`

## Usage

```lua
local modem = require("network.modem")
```

## API

### modem.info(interface)

- Description: Return packet and byte counters for a named modem-backed interface such as `eth0`.
- Parameters:
  - `interface`

```lua
modem.info(0)
```

### modem.send(handle, interface, destination, data)

- Description: Send a payload to a destination modem address, or short-circuit it locally when the destination is the interface's own modem.
- Parameters:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
modem.send(nil, 0, nil, nil)
```

### modem.start(eventHandler)

- Description: Scan modem components, register `eth*` interfaces, open port `1`, broadcast discovery packets, and attach a modem-message listener.
- Parameters:
  - `eventHandler`

```lua
modem.start(nil)
```

## Notes

- Only exported module functions are listed on this page.
