# term

## Summary

Terminal interaction helpers for cursor control, line editing, and screen-aware text output.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\term.lua`

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

### term.clearTab()

- Description: Clear the tab stop at the current cursor position.

```lua
term.clearTab()
```

### term.clearTabs()

- Description: Clear every configured tab stop.

```lua
term.clearTabs()
```

### term.copy(row, col, height, width, destRow, destCol)

- Description: Copy one rectangular screen region into another location using the Plan9k terminal extension.
- Parameters:
  - `row`
  - `col`
  - `height`
  - `width`
  - `destRow`
  - `destCol`

```lua
term.copy(nil, nil, nil, nil, nil, nil)
```

### term.cursorBackward(count)

- Description: Move the terminal cursor left by the requested number of columns.
- Parameters:
  - `count`

```lua
term.cursorBackward(nil)
```

### term.cursorDown(count)

- Description: Move the terminal cursor downward by the requested number of rows.
- Parameters:
  - `count`

```lua
term.cursorDown(nil)
```

### term.cursorForward(count)

- Description: Move the terminal cursor right by the requested number of columns.
- Parameters:
  - `count`

```lua
term.cursorForward(nil)
```

### term.cursorUp(count)

- Description: Move the terminal cursor upward by the requested number of rows.
- Parameters:
  - `count`

```lua
term.cursorUp(nil)
```

### term.disableLineWrap()

- Description: Disable automatic line wrapping when text reaches the viewport edge.

```lua
term.disableLineWrap()
```

### term.enableLineWrap()

- Description: Enable automatic line wrapping when text reaches the viewport edge.

```lua
term.enableLineWrap()
```

### term.enableScroll()

- Description: Enable scrolling over the whole display area.

```lua
term.enableScroll()
```

### term.eraseDown()

- Description: Erase from the current cursor row down to the bottom of the display.

```lua
term.eraseDown()
```

### term.eraseEndOfLine()

- Description: Erase from the current cursor position to the end of the line.

```lua
term.eraseEndOfLine()
```

### term.eraseLine()

- Description: Erase the whole current line.

```lua
term.eraseLine()
```

### term.eraseStartOfLine()

- Description: Erase from the current cursor position back to the start of the line.

```lua
term.eraseStartOfLine()
```

### term.eraseUp()

- Description: Erase from the current cursor row up to the top of the display.

```lua
term.eraseUp()
```

### term.fill(row, col, height, width)

- Description: Fill one rectangular screen region using the Plan9k terminal extension.
- Parameters:
  - `row`
  - `col`
  - `height`
  - `width`

```lua
term.fill(nil, nil, nil, nil)
```

### term.forceCursorPosition(row, col)

- Description: Set the cursor position using the VT100 `f` positioning sequence.
- Parameters:
  - `row`
  - `col`

```lua
term.forceCursorPosition(nil, nil)
```

### term.getCursor()

- Description: Return the current cursor column and row.

```lua
term.getCursor()
```

### term.getCursorBlink(enabled)

- Description: Return whether cursor blinking is currently enabled.
- Parameters:
  - `enabled`

```lua
term.getCursorBlink(nil)
```

### term.getInfo()

- Description: Query the Plan9k terminal backend for the active GPU and screen addresses.

```lua
term.getInfo()
```

### term.getResolution()

- Description: Return the current terminal width and height in character cells.

```lua
term.getResolution()
```

### term.gpu()

- Description: Return the GPU proxy associated with the current Plan9k terminal session.

```lua
term.gpu()
```

### term.isAvailable()

- Description: Return whether the current system has a usable GPU and screen pair for terminal output.

```lua
term.isAvailable()
```

### term.read(history, dobreak, hint, pwchar)

- Description: Read an editable input line with optional history, completion hints, password masking, or validation filtering.
- Parameters:
  - `history`
  - `dobreak`
  - `hint`
  - `pwchar`

```lua
term.read(nil, nil, 0, nil)
```

### term.reset()

- Description: Restore the terminal to a default full-screen state, including colors and resolution when supported.

```lua
term.reset()
```

### term.resetCursor()

- Description: Move the cursor back to the home position at the upper-left corner.

```lua
term.resetCursor()
```

### term.restoreCursor()

- Description: Restore the cursor position saved by `saveCursor`.

```lua
term.restoreCursor()
```

### term.restoreCursorAndAttr()

- Description: Restore both cursor position and text attributes saved by `saveCursorAndAttr`.

```lua
term.restoreCursorAndAttr()
```

### term.saveCursor()

- Description: Save the current cursor position for later restoration.

```lua
term.saveCursor()
```

### term.saveCursorAndAttr()

- Description: Save the current cursor position together with text attributes.

```lua
term.saveCursorAndAttr()
```

### term.scrollScreen(from,to)

- Description: Restrict scrolling to a specific row range on the terminal display.
- Parameters:
  - `from`
  - `to`

```lua
term.scrollScreen(nil, nil)
```

### term.scrollScreenDown()

- Description: Scroll the display content down by one line.

```lua
term.scrollScreenDown()
```

### term.scrollScreenUp()

- Description: Scroll the display content up by one line.

```lua
term.scrollScreenUp()
```

### term.setAttr(attr)

- Description: Apply one text attribute mode such as reset, bright, blink, or reverse video.
- Parameters:
  - `attr`

```lua
term.setAttr(nil)
```

### term.setBackground(color)

- Description: Set the terminal background color using the ANSI color palette.
- Parameters:
  - `color`

```lua
term.setBackground(nil)
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

### term.setCursorPosition(row,col)

- Description: Set the cursor to a specific row and column using the VT100 `H` positioning sequence.
- Parameters:
  - `row`
  - `col`

```lua
term.setCursorPosition(nil, nil)
```

### term.setForeground(color)

- Description: Set the terminal foreground color using the ANSI color palette.
- Parameters:
  - `color`

```lua
term.setForeground(nil)
```

### term.setResolution(height,width)

- Description: Request a new terminal resolution through the Plan9k terminal extension.
- Parameters:
  - `height`
  - `width`

```lua
term.setResolution(nil, nil)
```

### term.tab()

- Description: Set a tab stop at the current cursor position.

```lua
term.tab()
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
