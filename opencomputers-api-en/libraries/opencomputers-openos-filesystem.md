# filesystem

## Summary

Path, mount, and file-handle helpers for OpenOS-style filesystems.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\filesystem.lua`

## Usage

```lua
local filesystem = require("filesystem")
```

## API

### filesystem.canonical(path)

- Description: Normalize a path by collapsing redundant separators and relative segments.
- Parameters:
  - `path`

```lua
filesystem.canonical(nil)
```

### filesystem.concat(...)

- Description: Join path fragments and normalize the result.
- Parameters:
  - `...`

```lua
filesystem.concat(nil)
```

### filesystem.exists(path)

- Description: Check whether a path exists.
- Parameters:
  - `path`

```lua
filesystem.exists(nil)
```

### filesystem.get(path)

- Description: Resolve a path to its backing filesystem proxy and the path inside that filesystem.
- Parameters:
  - `path`

```lua
filesystem.get(nil)
```

### filesystem.isDirectory(path)

- Description: Check whether a path refers to a directory.
- Parameters:
  - `path`

```lua
filesystem.isDirectory(nil)
```

### filesystem.list(path)

- Description: List entries in a directory, including virtual mount entries when applicable.
- Parameters:
  - `path`

```lua
filesystem.list(nil)
```

### filesystem.mount(fs, path)

- Description: Mount a filesystem proxy at the specified virtual path.
- Parameters:
  - `fs`
  - `path`

```lua
filesystem.mount(nil, nil)
```

### filesystem.name(path)

- Description: Return the final path segment.
- Parameters:
  - `path`

```lua
filesystem.name(nil)
```

### filesystem.open(path, mode)

- Description: Open a file and return a wrapper that forwards handle operations to the backing filesystem.
- Parameters:
  - `path`
  - `mode`

```lua
filesystem.open(nil, nil)
```

### filesystem.path(path)

- Description: Return the directory portion of a path.
- Parameters:
  - `path`

```lua
filesystem.path(nil)
```

### filesystem.proxy(filter, options)

- Description: Resolve a filesystem address or filter into a filesystem proxy.
- Parameters:
  - `filter`
  - `options`

```lua
filesystem.proxy(nil, nil)
```

### filesystem.realPath(path)

- Description: Resolve symbolic links and mount boundaries to produce a concrete filesystem path.
- Parameters:
  - `path`

```lua
filesystem.realPath(nil)
```

## Notes

- This page documents exported `filesystem.*` functions only.
- Internal helpers such as path segmentation or mount-tree traversal are intentionally omitted from the public API list.
