# serialization

## Summary

Convert Lua values to source-like text and restore them back into Lua values.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\serialization.lua`

## Usage

```lua
local serialization = require("serialization")
```

## API

### serialization.serialize(value, pretty)

- Description: Turn numbers, strings, booleans, nil, and tables into Lua-literal-like text, with optional pretty formatting.
- Parameters:
  - `value`
  - `pretty`

```lua
serialization.serialize(nil, nil)
```

### serialization.unserialize(data)

- Description: Evaluate serialized text inside a restricted environment and return the reconstructed Lua value.
- Parameters:
  - `data`

```lua
serialization.unserialize(nil)
```

## Notes

- Pretty mode is convenient for display, but it may emit output that cannot be round-tripped back through `unserialize`.
