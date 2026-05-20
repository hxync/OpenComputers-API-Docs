# io

## Summary

High-level file descriptor, stream, and pipe helpers layered on top of filesystem streams.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\io.lua`

## Usage

```lua
local io = require("io")
```

## API

### io.close(file)

- Description: Close the supplied file handle, or the current output stream when no handle is passed.
- Parameters:
  - `file`

```lua
io.close(nil)
```

### io.dup(fd)

- Description: Create a lightweight duplicate wrapper around an existing file descriptor object.
- Parameters:
  - `fd`

```lua
io.dup(nil)
```

### io.error(file)

- Description: Get or replace the process standard error stream.
- Parameters:
  - `file`

```lua
io.error(nil)
```

### io.flush()

- Description: Flush the current standard output stream.

```lua
io.flush()
```

### io.input(file)

- Description: Get or replace the process standard input stream.
- Parameters:
  - `file`

```lua
io.input(nil)
```

### io.lines(filename, ...)

- Description: Return an iterator over file records, either from a named file or the current input stream.
- Parameters:
  - `filename`
  - `...`

```lua
io.lines("value", nil)
```

### io.open(path, mode)

- Description: Open a filesystem path and wrap the returned handle in a buffered stream object.
- Parameters:
  - `path`
  - `mode`

```lua
io.open(nil, nil)
```

### io.output(file)

- Description: Get or replace the process standard output stream.
- Parameters:
  - `file`

```lua
io.output(nil)
```

### io.popen(prog, mode, env)

- Description: Open a pipe to another process or command pipeline.
- Parameters:
  - `prog`
  - `mode`
  - `env`

```lua
io.popen(nil, nil, nil)
```

### io.read(...)

- Description: Read from the current standard input stream using Lua-style format arguments.
- Parameters:
  - `...`

```lua
io.read(nil)
```

### io.stream(fd,file,mode)

- Description: Fetch or replace one of the numeric standard streams stored in the process I/O table.
- Parameters:
  - `fd`
  - `file`
  - `mode`

```lua
io.stream(nil, nil, nil)
```

### io.tmpfile()

- Description: Create and open a temporary file using `os.tmpname()`.

```lua
io.tmpfile()
```

### io.type(object)

- Description: Report whether a value is an open file, a closed file, or not a file object.
- Parameters:
  - `object`

```lua
io.type(nil)
```

### io.write(...)

- Description: Write values to the current standard output stream.
- Parameters:
  - `...`

```lua
io.write(nil)
```

## Notes

- OpenOS and Plan9k expose compatible `io` tables that route standard streams through per-process handle state.
