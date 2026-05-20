# buffer

## Summary

Extended buffered-stream helpers for seekable reads, formatted reads, and buffered writes.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_buffer.lua`

## Usage

```lua
local buffer = require("buffer")  -- advanced methods load lazily
```

## API

### buffer.buffered_write(arg)

- Description: Write one string through the configured buffering strategy, flushing first when needed.
- Parameters:
  - `arg`: String chunk to append or flush through the wrapped stream.

```lua
stream:buffered_write('hello\n')
```

### buffer.formatted_read(readChunk, ...)

- Description: Read one or more values using Lua-style numeric and `*l/*L/*a/*n` format specifiers.
- Parameters:
  - `readChunk`: Low-level chunk reader callback used to fill the internal read buffer.
  - `...`: Format specifiers or numeric byte/character counts.

```lua
local line = stream:formatted_read(readChunk, '*l')
```

### buffer.getTimeout()

- Description: Return the configured read timeout in seconds, or `nil` when no timeout is active.

```lua
local timeout = stream:getTimeout()
```

### buffer.readAll(readChunk)

- Description: Read until EOF and return all buffered and newly fetched data as one string.
- Parameters:
  - `readChunk`: Low-level chunk reader callback.

```lua
local body = stream:readAll(readChunk)
```

### buffer.readBytesOrChars(readChunk, n)

- Description: Read exactly the requested number of bytes or Unicode characters, depending on binary mode.
- Parameters:
  - `readChunk`: Low-level chunk reader callback.
  - `n`: Requested byte or character count.

```lua
local chunk = stream:readBytesOrChars(readChunk, 16)
```

### buffer.readNumber(readChunk)

- Description: Skip leading whitespace, then parse and return the next readable numeric token.
- Parameters:
  - `readChunk`: Low-level chunk reader callback.

```lua
local number = stream:readNumber(readChunk)
```

### buffer.seek(whence, offset)

- Description: Flush pending writes if needed, adjust read buffering state, and seek the wrapped stream.
- Parameters:
  - `whence`: Seek mode: `set`, `cur`, or `end`.
  - `offset`: Integer offset relative to the chosen mode.

```lua
stream:seek('set', 0)
```

### buffer.setTimeout(value)

- Description: Set the buffered stream read timeout in seconds.
- Parameters:
  - `value`: Timeout value that will be converted with `tonumber`.

```lua
stream:setTimeout(2)
```

### buffer.size()

- Description: Return the unread size currently visible through the buffered wrapper, including internal buffered data.

```lua
local remaining = stream:size()
```

## Notes

- Only exported module functions are listed on this page.
