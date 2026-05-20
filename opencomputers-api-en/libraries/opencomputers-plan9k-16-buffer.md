# buffer

## Summary

Plan9k buffered stream helpers, including an in-memory pipe pair used for pseudo-terminals and spawned programs.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\16_buffer.lua`

## Usage

```lua
local buffer = require("buffer")
```

## API

### buffer.close()

- Description: Flush pending buffered output when needed, mark the wrapper closed, and close the wrapped stream.

```lua
buffer.close()
```

### buffer.flush()

- Description: Write the current buffered output block to the wrapped stream immediately.

```lua
buffer.flush()
```

### buffer.getTimeout()

- Description: Return the configured read timeout in seconds for this buffered stream.

```lua
buffer.getTimeout()
```

### buffer.lines(...)

- Description: Return an iterator that repeatedly reads records from the buffered stream using the provided read formats.
- Parameters:
  - `...`

```lua
buffer.lines(nil)
```

### buffer.new(mode, stream)

- Description: Wrap one raw stream handle with Plan9k buffering state, read buffer, write buffer, and mode flags.
- Parameters:
  - `mode`
  - `stream`

```lua
buffer.new(nil, nil)
```

### buffer.pipe()

- Description: Create a connected buffered reader and writer pair backed by an in-memory queue and thread wake-up signals.

```lua
local input, output = buffer.pipe()
```

### buffer.read(...)

- Description: Read data from the wrapped stream using Lua-style read formats while honoring the configured timeout.
- Parameters:
  - `...`

```lua
buffer.read(nil)
```

### buffer.seek(whence, offset)

- Description: Flush writes if needed, adjust buffered read state, and reposition the wrapped stream.
- Parameters:
  - `whence`
  - `offset`

```lua
buffer.seek(nil, nil)
```

### buffer.setTimeout(value)

- Description: Set the read timeout in seconds for subsequent buffered reads.
- Parameters:
  - `value`

```lua
buffer.setTimeout(nil)
```

### buffer.setvbuf(mode, size)

- Description: Set the buffering mode and buffer size for future writes.
- Parameters:
  - `mode`
  - `size`

```lua
buffer.setvbuf(nil, nil)
```

### buffer.write(...)

- Description: Write strings or numbers through the buffered wrapper while respecting the active buffering mode.
- Parameters:
  - `...`

```lua
buffer.write(nil)
```

## Notes

- Only exported module functions are listed on this page.
