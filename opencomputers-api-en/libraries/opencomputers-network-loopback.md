# loopback

## Summary

Local loopback network driver that feeds packets back into the bundled software network stack.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\loopback.lua`

## Usage

```lua
local loopback = require("network.loopback")
```

## API

### loopback.info(interface)

- Description: Return packet and byte counters for the loopback interface.
- Parameters:
  - `interface`

```lua
loopback.info(0)
```

### loopback.send(handle, interface, destination, data)

- Description: Deliver a packet to the loopback handler when the destination is `localhost` on interface `lo`.
- Parameters:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
loopback.send(nil, 0, nil, nil)
```

### loopback.start(eventHandler)

- Description: Register the `lo` interface and its `localhost` host entry with the supplied event handler.
- Parameters:
  - `eventHandler`

```lua
loopback.start(nil)
```

## Notes

- Only exported module functions are listed on this page.
