# core_cursor

## Summary

Additional cursor helpers for mouse positioning, tab completion, and horizontal no-wrap editing.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_cursor.lua`

## Usage

```lua
local core_cursor = require("core/cursor")  -- loads horizontal helpers lazily
```

## API

### core_cursor.tab(cursor)

- Description: Cycle through completion candidates supplied by the cursor's `hint` callback, using Shift+Tab to move backward.
- Parameters:
  - `cursor`: Cursor object whose `hint` field returns completion candidates.

```lua
core_cursor.tab(cursor)
```

### core_cursor.touch(cursor, gx, gy)

- Description: Move the editing cursor toward the character nearest the touched screen cell.
- Parameters:
  - `cursor`: Cursor object to reposition.
  - `gx`: Touched screen X coordinate in global screen space.
  - `gy`: Touched screen Y coordinate in global screen space.

```lua
core_cursor.touch(cursor, x, y)
```

## Notes

- These helpers are attached on demand when the base cursor module loads `/lib/core/full_cursor.lua` through `package.delay`.
