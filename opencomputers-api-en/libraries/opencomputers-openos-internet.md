# internet

## Summary

Open HTTP requests and buffered socket streams through the primary internet card.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\internet.lua`

## Usage

```lua
local internet = require("internet")
```

## API

### internet.open(address, port)

- Description: Connect to a host or `host:port` pair and wrap the socket in a buffered read-write stream.
- Parameters:
  - `address`
  - `port`

```lua
internet.open("address", nil)
```

### internet.request(url, data, headers, method)

- Description: Start an HTTP request and return an iterator-like object that yields response chunks.
- Parameters:
  - `url`
  - `data`
  - `headers`
  - `method`

```lua
internet.request(nil, nil, nil, nil)
```

### internet.socket(address, port)

- Description: Open a raw socket-like stream object backed by the primary internet card.
- Parameters:
  - `address`
  - `port`

```lua
internet.socket("address", nil)
```

## Notes

- Only exported module functions are listed on this page.
