# data

## Summary

Binary-to-hex helpers plus passthrough access to the primary data card component.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\lib\data.lua`

## Usage

```lua
local data = require("data")
```

## API

### data.fromHex(hex)

- Description: Convert hexadecimal text into raw binary bytes.
- Parameters:
  - `hex`

```lua
data.fromHex(nil)
```

### data.toHex(data)

- Description: Convert raw binary bytes into uppercase hexadecimal text.
- Parameters:
  - `data`

```lua
data.toHex(nil)
```

## Notes

- Besides the documented helpers on this page, unknown field lookups fall through to `component.data`, so additional data-card methods remain available through the same module.
