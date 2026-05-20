# filesystem

## Summary

Path, mount, and file-handle helpers for OpenOS-style filesystems.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_filesystem.lua`

## Usage

```lua
local filesystem = require("filesystem")
```

## API

### filesystem.copy(fromPath, toPath)

- Description: Copy a file or directory tree to a new path.
- Parameters:
  - `fromPath`
  - `toPath`

```lua
filesystem.copy(nil, nil)
```

### filesystem.isAutorunEnabled()

- Description: Return whether automatic execution of autorun entries is enabled.

```lua
filesystem.isAutorunEnabled()
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

### filesystem.makeDirectory(path)

- Description: Create a directory and any required intermediate structure supported by the implementation.
- Parameters:
  - `path`

```lua
filesystem.makeDirectory(nil)
```

### filesystem.mounts()

- Description: Iterate currently mounted filesystems and their mount points.

```lua
filesystem.mounts()
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
