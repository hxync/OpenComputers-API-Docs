# filesystem

## Summary

Path, mount, and file-handle helpers for OpenOS-style filesystems.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\05_vfs.lua`

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

### filesystem.concat(pathA, pathB, ...)

- Description: Join path fragments and normalize the result.
- Parameters:
  - `pathA`
  - `pathB`
  - `...`

```lua
filesystem.concat(nil, nil, nil)
```

### filesystem.copy(fromPath, toPath)

- Description: Copy a file or directory tree to a new path.
- Parameters:
  - `fromPath`
  - `toPath`

```lua
filesystem.copy(nil, nil)
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

### filesystem.isAutorunEnabled()

- Description: Return whether automatic execution of autorun entries is enabled.

```lua
filesystem.isAutorunEnabled()
```

### filesystem.isDirectory(path)

- Description: Check whether a path refers to a directory.
- Parameters:
  - `path`

```lua
filesystem.isDirectory(nil)
```

### filesystem.isLink(path)

- Description: Return whether a path currently resolves to a symbolic link.
- Parameters:
  - `path`

```lua
filesystem.isLink(nil)
```

### filesystem.lastModified(path)

- Description: Return the last modification timestamp for a path.
- Parameters:
  - `path`

```lua
filesystem.lastModified(nil)
```

### filesystem.link(target, linkpath)

- Description: Create a symbolic link entry.
- Parameters:
  - `target`
  - `linkpath`

```lua
filesystem.link(nil, nil)
```

### filesystem.list(path)

- Description: List entries in a directory, including virtual mount entries when applicable.
- Parameters:
  - `path`

```lua
filesystem.list(nil)
```

### filesystem.makeDirectory(path)

- Description: Create a directory and any required intermediate structure supported by the implementation.
- Parameters:
  - `path`

```lua
filesystem.makeDirectory(nil)
```

### filesystem.mount(fs, path)

- Description: Mount a filesystem proxy at the specified virtual path.
- Parameters:
  - `fs`
  - `path`

```lua
filesystem.mount(nil, nil)
```

### filesystem.mounts()

- Description: Iterate currently mounted filesystems and their mount points.

```lua
filesystem.mounts()
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

### filesystem.proxy(filter)

- Description: Resolve a filesystem address or filter into a filesystem proxy.
- Parameters:
  - `filter`

```lua
filesystem.proxy(nil)
```

### filesystem.remove(path)

- Description: Remove a file, directory, or removable virtual entry.
- Parameters:
  - `path`

```lua
filesystem.remove(nil)
```

### filesystem.rename(oldPath, newPath)

- Description: Rename or move an entry to a new path.
- Parameters:
  - `oldPath`
  - `newPath`

```lua
filesystem.rename(nil, nil)
```

### filesystem.segments(path)

- Description: Split a path into normalized segments.
- Parameters:
  - `path`

```lua
filesystem.segments(nil)
```

### filesystem.setAutorunEnabled(value)

- Description: Enable or disable automatic execution of autorun entries.
- Parameters:
  - `value`

```lua
filesystem.setAutorunEnabled(nil)
```

### filesystem.size(path)

- Description: Return the size of a file in bytes.
- Parameters:
  - `path`

```lua
filesystem.size(nil)
```

### filesystem.umount(fsOrPath)

- Description: Unmount a filesystem by proxy, address, or mount path.
- Parameters:
  - `fsOrPath`

```lua
filesystem.umount(nil)
```

## Notes

- This page documents exported `filesystem.*` functions only.
- Internal helpers such as path segmentation or mount-tree traversal are intentionally omitted from the public API list.
