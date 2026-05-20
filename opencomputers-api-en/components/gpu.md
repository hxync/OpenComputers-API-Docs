# gpu

## Summary

The `gpu` component is OpenComputers' text-mode graphics API. It controls screen binding, colors, palette entries, resolution, viewport size, text output, rectangular copy and fill operations, and off-screen VRAM buffers.

Many methods operate on the **currently active buffer**:

- buffer `0` is always the bound screen,
- buffers `1+` are GPU-allocated VRAM buffers.

If the active target is the real screen, operations consume normal GPU energy and call budget. When the active target is an off-screen buffer, many write operations are cheaper and can work even without a screen bound.

## Availability

- Repository: `opencomputers`
- Component name: `gpu`
- Typical host: computer, robot, tablet, microcontroller with a graphics card installed

## Usage

```lua
local component = require("component")
local gpu = component.gpu

if not gpu then
  error("gpu not installed")
end
```

## Core Concepts

- `bind(address[, reset])` connects the GPU to a screen.
- `setActiveBuffer(index)` chooses whether drawing goes to the screen (`0`) or a VRAM buffer (`1+`).
- `bitblt(...)` copies rectangular regions between buffers, including from VRAM back onto the screen.
- Coordinates passed to Lua are `1`-based.

## Buffer Management

### getActiveBuffer

- Syntax: `gpu.getActiveBuffer()`
- Returns: `number`
- Purpose: Gets the currently selected target buffer index.

`0` is always the screen slot and may be active even when no screen is currently bound.

### setActiveBuffer

- Syntax: `gpu.setActiveBuffer(index)`
- Returns:
  - Success: previous buffer index
  - Failure: `nil, reason`
- Purpose: Selects which buffer future drawing calls operate on.

Parameters:

- `index:number`: `0` for the bound screen, or a previously allocated VRAM buffer index.

Failure reason from source: `"invalid buffer index"`.

### buffers

- Syntax: `gpu.buffers()`
- Returns: `table`
- Purpose: Lists the currently allocated VRAM buffer indexes.

### allocateBuffer

- Syntax: `gpu.allocateBuffer([width, height])`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Allocates a new off-screen VRAM buffer and returns its index.

Parameters:

- `width:number` optional: buffer width. Defaults to the GPU's maximum width.
- `height:number` optional: buffer height. Defaults to the GPU's maximum height.

Behavior:

- Buffer `0` is reserved for the screen, so allocated buffers always start at `1`.
- A buffer may be allocated even when no screen is currently bound.
- Failure reasons from source include:
  - `"invalid page dimensions: must be greater than zero"`
  - `"not enough video memory"`
  - `"graphics card appears disconnected"`

### freeBuffer

- Syntax: `gpu.freeBuffer([index])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Frees one VRAM buffer.

Parameters:

- `index:number` optional: buffer index to free. Defaults to the current active buffer.

If the active buffer is freed, the active index falls back to `0`.

Failure reason from source: `"no buffer at index"`.

### freeAllBuffers

- Syntax: `gpu.freeAllBuffers()`
- Returns: `number`
- Purpose: Frees every VRAM buffer and returns how many were removed.

If the active buffer was one of the removed VRAM buffers, the active index becomes `0`.

### totalMemory

- Syntax: `gpu.totalMemory()`
- Returns: `number`
- Purpose: Gets total VRAM capacity available for GPU buffers.

This does not include the screen itself.

### freeMemory

- Syntax: `gpu.freeMemory()`
- Returns: `number`
- Purpose: Gets free VRAM capacity not currently used by allocated buffers.

### getBufferSize

- Syntax: `gpu.getBufferSize([index])`
- Returns:
  - Success: `number, number`
  - Failure: `nil, reason`
- Purpose: Gets the width and height of a buffer.

Parameters:

- `index:number` optional: buffer index to query. Defaults to the active buffer.

Notes:

- Passing `0` returns the bound screen resolution.
- Failure reason from source: `"invalid buffer index"`.

Example:

```lua
local id = gpu.allocateBuffer(80, 25)
print("Allocated buffer:", id)
print("Size:", gpu.getBufferSize(id))
```

## Buffer Copying

### bitblt

- Syntax: `gpu.bitblt([dst, col, row, width, height, src, fromCol, fromRow])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Copies a rectangular region from one buffer to another.

Parameters:

- `dst:number` optional: destination buffer index. Defaults to screen buffer `0`.
- `col:number` optional: destination column. Defaults to `1`.
- `row:number` optional: destination row. Defaults to `1`.
- `width:number` optional: rectangle width. Defaults to the destination width.
- `height:number` optional: rectangle height. Defaults to the destination height.
- `src:number` optional: source buffer index. Defaults to the current active buffer.
- `fromCol:number` optional: source column. Defaults to `1`.
- `fromRow:number` optional: source row. Defaults to `1`.

Behavior:

- This is the main way to render an off-screen VRAM buffer onto the bound screen.
- Copying from one VRAM buffer to another is effectively memory-to-memory.
- Copying a dirty VRAM buffer to the screen can be expensive and may trigger call-budget throttling.
- Failure reasons come from the underlying buffer lookup, most commonly:
  - `"no screen"`
  - `"invalid buffer index"`
  - `"not enough energy"`

Example:

```lua
local page = assert(gpu.allocateBuffer(40, 10))
gpu.setActiveBuffer(page)
gpu.fill(1, 1, 40, 10, " ")
gpu.set(2, 2, "Prepared off-screen")
gpu.bitblt(0, 1, 1, 40, 10, page, 1, 1)
gpu.setActiveBuffer(0)
```

## Screen Binding

### bind

- Syntax: `gpu.bind(address[, reset])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Binds the GPU to a screen component.

Parameters:

- `address:string`: screen component address.
- `reset:boolean` optional: defaults to `true`.

Behavior when `reset` is `true`:

- resolution is reset to the minimum of GPU max resolution and screen max resolution,
- color depth is reset to the best shared depth,
- foreground becomes white,
- background becomes black,
- VRAM pages registered with the screen-side rasterizer are cleared.

Failure reasons from source:

- `"invalid address"`
- `"not a screen"`

### getScreen

- Syntax: `gpu.getScreen()`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Gets the address of the currently bound screen.

Failure reason from source: `"no screen"`.

## Color and Palette

### getBackground

- Syntax: `gpu.getBackground()`
- Returns: `number, boolean`
- Purpose: Gets the current background color and whether that color comes from the palette.

### setBackground

- Syntax: `gpu.setBackground(value[, palette])`
- Returns: `number[, number]`
- Purpose: Sets the current background color for future drawing operations.

Parameters:

- `value:number`: raw RGB color or palette index, depending on `palette`.
- `palette:boolean` optional: when `true`, interpret `value` as a palette index.

Return values:

- first result: previous resolved RGB color,
- second result: previous palette index when the old color came from the palette, otherwise `nil`.

### getForeground

- Syntax: `gpu.getForeground()`
- Returns: `number, boolean`
- Purpose: Gets the current foreground color and whether that color comes from the palette.

### setForeground

- Syntax: `gpu.setForeground(value[, palette])`
- Returns: `number[, number]`
- Purpose: Sets the current foreground color for future drawing operations.

Parameters and return values behave the same way as `setBackground`.

### getPaletteColor

- Syntax: `gpu.getPaletteColor(index)`
- Returns: `number`
- Purpose: Reads one palette entry.

Parameter:

- `index:number`: palette slot index.

Invalid indices raise `"invalid palette index"`.

### setPaletteColor

- Syntax: `gpu.setPaletteColor(index, color)`
- Returns: `number`
- Purpose: Changes one palette entry and returns the previous color.

Parameters:

- `index:number`: palette slot index.
- `color:number`: new RGB color.

Invalid indices raise `"invalid palette index"`.

### getDepth

- Syntax: `gpu.getDepth()`
- Returns: `number`
- Purpose: Gets the currently active color depth in bits.

### setDepth

- Syntax: `gpu.setDepth(depth)`
- Returns: previous depth
- Purpose: Changes the color depth.

Supported values depend on GPU tier and the bound screen. Source accepts:

- `1`
- `4`
- `8`

Unsupported requests raise `"unsupported depth"`.

### maxDepth

- Syntax: `gpu.maxDepth()`
- Returns: `number`
- Purpose: Gets the highest color depth supported by both the GPU and the bound screen.

Example:

```lua
local oldBg = gpu.setBackground(0x0000FF)
local oldFg = gpu.setForeground(0xFFFFFF)
print("Previous colors:", oldBg, oldFg)
```

## Resolution and Viewport

### getResolution

- Syntax: `gpu.getResolution()`
- Returns: `number, number`
- Purpose: Gets the current text resolution of the active screen buffer.

### setResolution

- Syntax: `gpu.setResolution(width, height)`
- Returns: `boolean`
- Purpose: Changes the screen resolution.

Parameters:

- `width:number`
- `height:number`

The request must fit both the GPU tier limits and the bound screen's limits. Invalid values raise `"unsupported resolution"`.

### maxResolution

- Syntax: `gpu.maxResolution()`
- Returns: `number, number`
- Purpose: Gets the best shared maximum resolution supported by both the GPU and the bound screen.

### getViewport

- Syntax: `gpu.getViewport()`
- Returns: `number, number`
- Purpose: Gets the current viewport size.

The viewport is the visible area used for rendering; it cannot exceed the current resolution.

### setViewport

- Syntax: `gpu.setViewport(width, height)`
- Returns: `boolean`
- Purpose: Changes the viewport size.

The requested viewport must fit inside both the GPU limits and the current screen resolution. Invalid values raise `"unsupported viewport size"`.

## Reading and Drawing

### get

- Syntax: `gpu.get(x, y)`
- Returns: `string, number, number, number|nil, number|nil`
- Purpose: Reads one cell from the active buffer.

Return values:

- character at the cell,
- resolved foreground RGB color,
- resolved background RGB color,
- foreground palette index if the foreground came from the palette, otherwise `nil`,
- background palette index if the background came from the palette, otherwise `nil`.

### set

- Syntax: `gpu.set(x, y, value[, vertical])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Writes a string into the active buffer.

Parameters:

- `x:number`, `y:number`: starting position.
- `value:string`: text to draw.
- `vertical:boolean` optional: when `true`, writes one code point per row.

Failure reason from source: `"not enough energy"`.

### copy

- Syntax: `gpu.copy(x, y, width, height, tx, ty)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Copies a rectangle inside the active buffer by the specified translation.

Parameters:

- `x:number`, `y:number`: source origin.
- `width:number`, `height:number`: source size.
- `tx:number`, `ty:number`: translation offset.

Failure reason from source: `"not enough energy"`.

### fill

- Syntax: `gpu.fill(x, y, width, height, char)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Fills a rectangle in the active buffer with one character.

Parameters:

- `char:string`: must contain exactly one Unicode code point.

Failure behaviors:

- returns `nil, "not enough energy"` when power is insufficient,
- raises `"invalid fill value"` when `char` is not exactly one character.

Example:

```lua
gpu.bind(component.screen.address)
gpu.setResolution(80, 25)
gpu.setBackground(0x000000)
gpu.setForeground(0x00FF00)
gpu.fill(1, 1, 80, 25, " ")
gpu.set(2, 2, "OpenComputers")
```

## Related

- `screen`
- `component`
- `computer`
