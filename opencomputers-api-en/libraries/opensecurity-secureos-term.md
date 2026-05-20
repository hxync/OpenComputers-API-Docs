# term

## Summary

Terminal interaction helpers for cursor control, line editing, and screen-aware text output.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\term.lua`

## Usage

```lua
local term = require("term")
```

## API

### term.clear()

- Description: Clear the full terminal area and reset the cursor to the top-left corner.

```lua
term.clear()
```

### term.clearLine()

- Description: Erase the current line and reset the cursor to its first column.

```lua
term.clearLine()
```

### term.getCursor()

- Description: Return the current cursor column and row.

```lua
term.getCursor()
```

### term.getCursorBlink()

- Description: Return whether cursor blinking is currently enabled.

```lua
term.getCursorBlink()
```

### term.isAvailable()

- Description: Return whether the current system has a usable GPU and screen pair for terminal output.

```lua
term.isAvailable()
```

### term.isWide(x, y)

- Description: Check whether the cell at a screen position belongs to a wide character.
- Parameters:
  - `x`
  - `y`

```lua
term.isWide(nil, nil)
```

### term.read(history, dobreak, hint, pwchar, filter)

- Description: Read an editable input line with optional history, completion hints, password masking, or validation filtering.
- Parameters:
  - `history`
  - `dobreak`
  - `hint`
  - `pwchar`
  - `filter`

```lua
term.read(nil, nil, 0, nil, nil)
```

### term.reset()

- Description: Restore the terminal to a default full-screen state, including colors and resolution when supported.

```lua
term.reset()
```

### term.setCursor(col, row)

- Description: Move the cursor to the specified column and row.
- Parameters:
  - `col`
  - `row`

```lua
term.setCursor(nil, nil)
```

### term.setCursorBlink(enabled)

- Description: Enable or disable cursor blinking.
- Parameters:
  - `enabled`

```lua
term.setCursorBlink(nil)
```

### term.write(value, wrap)

- Description: Write text to the terminal, optionally wrapping lines to the viewport width.
- Parameters:
  - `value`
  - `wrap`

```lua
term.write(nil, nil)
```

## Notes

- Different systems expose different subsets of the terminal API, but all variants revolve around cursor state, interactive reading, and writing through the active screen and GPU pair.
