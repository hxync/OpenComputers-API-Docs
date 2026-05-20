# vt100

## Summary

Minimal VT100 escape-sequence parser used by the TTY writer to update cursor and color state.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\vt100.lua`

## Usage

```lua
local vt100 = require("vt100")
```

## API

### vt100.parse(window)

- Description: Consume one VT100 escape sequence from `window.output_buffer`, apply its side effects, and return printable fallback text when parsing fails.
- Parameters:
  - `window`: TTY window state table containing the output buffer, cursor, GPU proxy, and saved attributes.

```lua
local passthrough = vt100.parse(tty.window)
```

## Notes

- The bootstrap parser handles common color and save/restore sequences first, then loads `/lib/core/full_vt.lua` lazily if it encounters more advanced control sequences.
