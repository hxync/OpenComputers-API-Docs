# io

## Summary

Exported `io` helpers discovered directly from bundled Lua source.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\17_io.lua`

## Usage

```lua
local io = require("io")
```

## API

### io.close(file)

- Description: Close the supplied file handle, or the current output stream when no handle is provided.
- Parameters:
  - `file`

```lua
io.close(nil)
```

### io.flush()

- Description: Flush the current output stream.

```lua
io.flush()
```

### io.input(file)

- Description: Get or replace the current thread's standard input stream, accepting either a file object or a path string.
- Parameters:
  - `file`

```lua
io.input(nil)
```

### io.lines(filename, ...)

- Description: Return an iterator that reads records either from a named file or from the current input stream.
- Parameters:
  - `filename`
  - `...`

```lua
io.lines("value", nil)
```

### io.open(path, mode)

- Description: Open a path through Plan9k VFS and wrap the resulting stream in a buffered file object.
- Parameters:
  - `path`
  - `mode`

```lua
io.open(nil, nil)
```

### io.output(file)

- Description: Get or replace the current thread's standard output stream, accepting either a file object or a path string.
- Parameters:
  - `file`

```lua
io.output(nil)
```

### io.pipe()

- Description: Return a buffered input/output pipe pair built on the Plan9k buffer module.

```lua
local input, output = io.pipe()
```

### io.popen(prog, mode, ...)

- Description: Spawn a program with connected pipe endpoints and return handles for reading, writing, or both depending on the selected mode.
- Parameters:
  - `prog`
  - `mode`
  - `...`

```lua
io.popen(nil, nil, nil)
```

### io.read(...)

- Description: Read from the current input stream using Lua-style format arguments.
- Parameters:
  - `...`

```lua
io.read(nil)
```

### io.tmpfile()

- Description: Create a temporary file via `os.tmpname()` and open it for appending.

```lua
io.tmpfile()
```

### io.type(object)

- Description: Report whether a value is an open file object, a closed file object, or not a file object at all.
- Parameters:
  - `object`

```lua
io.type(nil)
```

### io.write(...)

- Description: Write values to the current output stream.
- Parameters:
  - `...`

```lua
io.write(nil)
```

## Notes

- Only exported module functions are listed on this page.
