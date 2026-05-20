# filesystem

## Summary

The `filesystem` component is OpenComputers' standard file and directory API. It provides labeled volumes, file metadata queries, directory operations, and handle-based file I/O.

Compared with the raw `drive` component, `filesystem` is the higher-level storage surface most Lua programs should use.

## Availability

- Repository: `opencomputers`
- Component name: `filesystem`
- Typical hosts: hard drives, floppy disks, raid volumes, temporary filesystems, mounted storage

## Important Notes

- Paths are sanitized internally. Attempts to escape the filesystem root with `..` are rejected.
- `"/"` and `"."` are normalized to the filesystem root.
- File handles belong to the computer that opened them. Using another machine's handle causes `"bad file descriptor"`.
- Supported open modes are:
  - `"r"` / `"rb"`
  - `"w"` / `"wb"`
  - `"a"` / `"ab"`

## Usage

```lua
local component = require("component")
local fs = component.filesystem

if not fs then
  error("filesystem component not available")
end
```

For a specific disk, use `component.proxy(address)`.

## Volume Information

### getLabel

- Syntax: `fs.getLabel()`
- Returns: `string` or `nil`
- Purpose: Reads the volume label.

### setLabel

- Syntax: `fs.setLabel(value)`
- Returns: `string`
- Purpose: Changes the volume label and returns the resulting label.

Possible error from source: `"drive does not support labeling"`.

### isReadOnly

- Syntax: `fs.isReadOnly()`
- Returns: `boolean`
- Purpose: Checks whether the filesystem is read-only.

### spaceTotal

- Syntax: `fs.spaceTotal()`
- Returns: `number`
- Purpose: Gets total filesystem capacity in bytes.

If the backend reports unlimited size, the source returns positive infinity.

### spaceUsed

- Syntax: `fs.spaceUsed()`
- Returns: `number`
- Purpose: Gets used filesystem capacity in bytes.

## Path Queries

### exists

- Syntax: `fs.exists(path)`
- Returns: `boolean`
- Purpose: Checks whether a path exists.

### size

- Syntax: `fs.size(path)`
- Returns: `number`
- Purpose: Gets the size of the file or directory entry at a path.

### isDirectory

- Syntax: `fs.isDirectory(path)`
- Returns: `boolean`
- Purpose: Checks whether the path points to a directory.

### lastModified

- Syntax: `fs.lastModified(path)`
- Returns: `number`
- Purpose: Gets the real-world modification timestamp of a path.

### list

- Syntax: `fs.list(path)`
- Returns: `table` or `nil`
- Purpose: Lists names inside a directory.

If the directory does not exist or cannot be listed by the backend, this returns `nil`.

Example:

```lua
local entries = fs.list("/")
if entries then
  for name in entries do
    print(name)
  end
end
```

## Directory and Path Mutation

### makeDirectory

- Syntax: `fs.makeDirectory(path)`
- Returns: `boolean`
- Purpose: Creates a directory and any missing parent directories.

Behavior:

- Returns `true` when creation succeeds.
- Returns `false` when the path already exists or creation fails.

### remove

- Syntax: `fs.remove(path)`
- Returns: `boolean`
- Purpose: Removes a file or recursively deletes a directory tree.

This component implementation descends into directories and deletes children before deleting the parent.

### rename

- Syntax: `fs.rename(from, to)`
- Returns: `boolean`
- Purpose: Renames or moves a path.

## File Handle Operations

### open

- Syntax: `fs.open(path[, mode])`
- Returns: `userdata`
- Purpose: Opens a file handle.

Parameters:

- `path:string`: path to open.
- `mode:string` optional: defaults to `"r"`.

Behavior:

- Raises `"unsupported mode"` for unknown modes.
- Raises `"too many open handles"` when the current machine exceeds the configured handle limit.
- The returned handle is userdata; keep it and reuse it for later operations.

Example:

```lua
local handle = fs.open("/hello.txt", "w")
fs.write(handle, "hello\n")
fs.close(handle)
```

### close

- Syntax: `fs.close(handle)`
- Returns: no direct return values
- Purpose: Closes an open file handle.

Possible error from source: `"bad file descriptor"`.

### read

- Syntax: `fs.read(handle, count)`
- Returns: `string` or `nil`
- Purpose: Reads up to `count` bytes from a file handle.

Parameters:

- `handle:userdata`
- `count:number`

Behavior:

- Returns `nil` on EOF.
- Read size is clamped to the configured maximum read buffer size.
- Possible errors include:
  - `"bad file descriptor"`
  - `"not enough energy"`

### seek

- Syntax: `fs.seek(handle, whence, offset)`
- Returns: `number`
- Purpose: Changes the file pointer and returns the new position.

Parameters:

- `handle:userdata`
- `whence:string`: `"set"`, `"cur"`, or `"end"`.
- `offset:number`

Unknown `whence` values raise `"invalid mode"`.

### write

- Syntax: `fs.write(handle, value)`
- Returns: `boolean`
- Purpose: Writes bytes to an open file handle.

Parameters:

- `handle:userdata`
- `value:string`

Behavior:

- Returns `true` on success.
- Possible errors include:
  - `"bad file descriptor"`
  - `"not enough energy"`

## File I/O Example

```lua
local handle = fs.open("/note.txt", "w")
fs.write(handle, "OpenComputers\n")
fs.close(handle)

handle = fs.open("/note.txt", "r")
print(fs.read(handle, 64))
fs.close(handle)
```

## Related

- `drive`
- `disk_drive`
- `component`
