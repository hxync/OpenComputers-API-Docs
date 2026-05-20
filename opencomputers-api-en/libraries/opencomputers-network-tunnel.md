# tunnel

## Summary

Software tunnel-network driver built on OpenComputers tunnel cards and the same discovery protocol as the modem backend.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\tunnel.lua`

## Usage

```lua
local tunnel = require("network.tunnel")
```

## API

### tunnel.info(interface)

- Description: Return packet and byte counters for a named tunnel-backed interface such as `tun0`.
- Parameters:
  - `interface`

```lua
tunnel.info(0)
```

### tunnel.send(handle, interface, destination, data)

- Description: Send a payload through a tunnel interface, or short-circuit it locally when the destination is the same tunnel card.
- Parameters:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
tunnel.send(nil, 0, nil, nil)
```

### tunnel.start(eventHandler)

- Description: Scan tunnel components, register `tun*` interfaces, announce them with discovery packets, and attach a shared modem-message listener.
- Parameters:
  - `eventHandler`

```lua
tunnel.start(nil)
```

## Notes

- Only exported module functions are listed on this page.
