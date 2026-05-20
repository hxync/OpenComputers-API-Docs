# data

## Summary

Binary-to-hex helpers plus passthrough access to the primary data card component.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\17_ipc.lua`

## Usage

```lua
local data = require("data")
```

## API

## Notes

- Besides the documented helpers on this page, unknown field lookups fall through to `component.data`, so additional data-card methods remain available through the same module.
