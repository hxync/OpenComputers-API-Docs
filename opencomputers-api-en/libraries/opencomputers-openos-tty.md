# tty

## Summary

Terminal viewport, cursor, keyboard, and write-stream helpers used underneath `term` and `io`.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tty.lua`

## Usage

```lua
local tty = require("tty")
```

## API

### tty.bind(gpu)

- Description: Bind terminal output to a specific GPU proxy and refresh cached viewport information.
- Parameters:
  - `gpu`: GPU proxy that should become the active terminal backend.

```lua
tty.bind(component.gpu)
```

### tty.clear()

- Description: Scroll the whole viewport clear and reset the logical cursor to the top-left corner.

```lua
tty.clear()
```

### tty.getCursor()

- Description: Return the current logical cursor column and row inside the viewport.

```lua
local x, y = tty.getCursor()
```

### tty.getViewport()

- Description: Return viewport width, height, offsets, and current cursor position.

```lua
local w, h, dx, dy, x, y = tty.getViewport()
```

### tty.gpu()

- Description: Return the GPU proxy currently bound to the terminal window.

```lua
local gpu = tty.gpu()
```

### tty.isAvailable()

- Description: Return whether the terminal has a bound GPU and that GPU is attached to a screen.

```lua
if tty.isAvailable() then tty.clear() end
```

### tty.keyboard()

- Description: Resolve the keyboard address associated with the active screen or fall back to the primary keyboard.

```lua
local keyboard = tty.keyboard()
```

### tty.screen()

- Description: Return the screen address currently attached to the bound GPU.

```lua
local screen = tty.screen()
```

### tty.setCursor(x, y)

- Description: Update the logical cursor position tracked by `tty.window`.
- Parameters:
  - `x`: Target column inside the viewport.
  - `y`: Target row inside the viewport.

```lua
tty.setCursor(1, 1)
```

### tty.setViewport(width, height, dx, dy, x, y)

- Description: Set a custom viewport rectangle and initial cursor position within the bound screen area.
- Parameters:
  - `width`: Viewport width in characters.
  - `height`: Viewport height in characters.
  - `dx`: Optional X offset from the screen origin.
  - `dy`: Optional Y offset from the screen origin.
  - `x`: Optional initial cursor column, defaulting to `1`.
  - `y`: Optional initial cursor row, defaulting to `1`.

```lua
tty.setViewport(40, 10, 0, 0, 1, 1)
```

## Notes

- The module operates on `tty.window`, which tracks the active GPU, viewport, cursor location, and buffered VT100 output state.
