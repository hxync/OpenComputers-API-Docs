# term

## Summary

Terminal interaction helpers for cursor control, line editing, and screen-aware text output.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\term.lua`

## Usage

```lua
local term = require("term")
```

## API

### term.bind(gpu, window)

- Description: Bind terminal output to a specific GPU, optionally scoped to a window object.
- Parameters:
  - `gpu`
  - `window`

```lua
term.bind(nil, nil)
```

### term.clearLine()

- Description: Erase the current line and reset the cursor to its first column.

```lua
term.clearLine()
```

### term.getCursorBlink()

- Description: Return whether cursor blinking is currently enabled.

```lua
term.getCursorBlink()
```

### term.getGlobalArea()

- Description: Return the absolute screen rectangle occupied by the current terminal viewport.

```lua
term.getGlobalArea()
```

### term.pull(...)

- Description: Wait for input or other events while keeping the terminal cursor state refreshed.
- Parameters:
  - `...`

```lua
term.pull(nil)
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
