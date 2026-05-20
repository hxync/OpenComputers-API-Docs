# buffer

## Summary

Buffered file-like stream helpers used by OpenOS and SecureOS I/O wrappers.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\buffer.lua`

## Usage

```lua
local buffer = require("buffer")
```

## API

### buffer.close()

- Description: Flush pending output when needed, mark the wrapper closed, and then close the underlying stream.

```lua
buffer.close()
```

### buffer.flush()

- Description: Write the current buffered output block to the underlying stream immediately.

```lua
buffer.flush()
```

### buffer.getTimeout()

- Description: Return the current read timeout in seconds used by buffered read helpers.

```lua
buffer.getTimeout()
```

### buffer.lines(...)

- Description: Return an iterator that repeatedly reads records from the buffered stream.
- Parameters:
  - `...`

```lua
buffer.lines(nil)
```

### buffer.new(mode, stream)

- Description: Wrap a raw stream handle with buffering, read timeout state, and read or write mode flags.
- Parameters:
  - `mode`
  - `stream`

```lua
buffer.new(nil, nil)
```

### buffer.read(...)

- Description: Read data using Lua-style format arguments or line mode when no format is supplied.
- Parameters:
  - `...`

```lua
buffer.read(nil)
```

### buffer.seek(whence, offset)

- Description: Flush pending writes if needed, reposition the wrapped stream, and discard cached unread data.
- Parameters:
  - `whence`
  - `offset`

```lua
buffer.seek(nil, nil)
```

### buffer.setTimeout(value)

- Description: Replace the per-stream read timeout used by higher-level buffered reads.
- Parameters:
  - `value`

```lua
buffer.setTimeout(nil)
```

### buffer.setvbuf(mode, size)

- Description: Configure buffering mode as `no`, `full`, or `line`, and optionally set the buffer size.
- Parameters:
  - `mode`
  - `size`

```lua
buffer.setvbuf(nil, nil)
```

### buffer.write(...)

- Description: Write strings or numbers to the stream, honoring the active buffering mode.
- Parameters:
  - `...`

```lua
buffer.write(nil)
```

## Notes

- Instances returned by `buffer.new` wrap a lower-level stream and add line buffering, formatted reads, timeout handling, and file-like helper methods.
