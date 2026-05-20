# mv

## Summary

Move or rename files, links, and directory trees.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mv.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mv`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mv`

## Syntax

```lua
mv [OPTIONS] SOURCE... DEST
```

## Purpose

Move one or more source paths to a new destination. The implementation reuses the shared transfer library, preserving symbolic links, allowing directory moves, and deleting the source path after a successful copy.

## Parameters

- `SOURCE...`: One or more files, links, or directories to move.
- `DEST`: Target file path or destination directory.

## Options

- `-f`: Overwrite existing files without prompting.
- `-i`: Prompt before overwriting an existing destination file unless `-f` is also supplied.
- `-n`: Do not overwrite an existing destination file.
- `-v`: Print each move action as it happens.
- `--skip=PATTERN`: Skip source paths whose resolved full path matches the given Lua pattern.
- `-h, --help`: Print the built-in usage help text.

## Examples

```lua
mv a.txt b.txt
```
Rename or move `a.txt` to `b.txt`.

```lua
mv a.txt b.txt /tmp
```
Move both `a.txt` and `b.txt` into `/tmp`.

```lua
mv -n /home/data /backup/data
```
Move `/home/data` only if `/backup/data` does not already exist.

## Notes

- Mount points cannot be moved.
- A source path ending with `.` or `..` is rejected as an invalid move source.
