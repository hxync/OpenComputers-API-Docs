# cp

## Summary

Copy files, symbolic links, or directory trees.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\cp.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cp`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cp`

## Syntax

```lua
cp [OPTIONS] SOURCE... DEST
```

## Purpose

Copy one or more source paths to a destination file or directory. Regular files are duplicated with `filesystem.copy`, directories require `-r`, and symbolic links can be preserved instead of dereferenced.

## Parameters

- `SOURCE...`: One or more source files, directories, or links to copy.
- `DEST`: Target file path or destination directory.

## Options

- `-i`: Prompt before overwriting an existing destination file. This takes precedence over `-n`.
- `-n`: Do not overwrite an existing destination file.
- `-r`: Copy directories recursively.
- `-u`: Copy only when the destination file is missing or differs from the source file.
- `-P`: Preserve symbolic links as links instead of copying their target contents.
- `-v`: Print each copy action as it happens.
- `-x`: When copying directories recursively, stay on the source file system and skip mounted child file systems.
- `--skip=PATTERN`: Skip source paths whose resolved full path matches the given Lua pattern.

## Examples

```lua
cp a.txt b.txt
```
Copy `a.txt` to a new file named `b.txt`.

```lua
cp a.txt b.txt /tmp
```
Copy both `a.txt` and `b.txt` into the `/tmp` directory.

```lua
cp -r /home/project /backup
```
Recursively copy the `/home/project` directory tree into `/backup`.

```lua
cp -n /etc/hostname /tmp/hostname
```
Copy the file only if `/tmp/hostname` does not already exist.

## Notes

- If a source path ends with `.` or `..`, the command copies the contents of that directory into the destination instead of nesting the directory itself.
- The destination filesystem must be writable; otherwise the copy fails before data transfer begins.
