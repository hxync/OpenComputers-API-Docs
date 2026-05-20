# core_cursor

## Summary

Editable input cursor state machine used by OpenOS terminal line reading.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\cursor.lua`

## Usage

```lua
local core_cursor = require("core/cursor")
```

## API

### core_cursor.new(base, index)

- Description: Create or reinitialize a cursor object, optionally extending a custom base table or alternate parent behavior.
- Parameters:
  - `base`: Optional existing table that should receive cursor fields and methods.
  - `index`: Optional fallback parent table to use as the metatable `__index` chain.

```lua
local cursor = require("core/cursor").new()
```

### core_cursor.read(cursor)

- Description: Run the interactive input loop until one complete line, EOF-like termination, or interruption is produced.
- Parameters:
  - `cursor`: Cursor object created by `core_cursor.new`.

```lua
local line, reason = require("core/cursor").read(cursor)
```

## Notes

- This module drives history, insertion, deletion, cursor movement, and clipboard-aware input handling for `tty.stream.read`.
