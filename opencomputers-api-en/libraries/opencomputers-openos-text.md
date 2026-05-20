# text

## Summary

String tokenization, wrapping, padding, and escape helpers for shell- and terminal-oriented text.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\text.lua`

## Usage

```lua
local text = require("text")
```

## API

### text.escapeMagic(txt)

- Description: Escape Lua pattern magic characters so a string can be matched literally.
- Parameters:
  - `txt`

```lua
text.escapeMagic(nil)
```

### text.removeEscapes(txt)

- Description: Undo the escaping added by `escapeMagic` for Lua pattern magic characters.
- Parameters:
  - `txt`

```lua
text.removeEscapes(nil)
```

### text.trim(value)

- Description: Remove leading and trailing whitespace from a string.
- Parameters:
  - `value`

```lua
text.trim(nil)
```

## Notes

- OpenOS exposes a small bootstrap subset first and delays the richer tokenization and wrapping helpers until the full module loads.
