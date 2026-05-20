# touch

## Summary

Update file modification times or create empty files.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\touch.lua`

## Syntax

```lua
touch [OPTIONS] FILE...
```

## Purpose

Open each named file in append mode so its modification time becomes current. When a path does not exist, the command creates an empty file unless `-c` or `--no-create` is supplied.

## Parameters

- `FILE...`: One or more file paths to update or create.

## Options

- `-c, --no-create`: Do not create files that do not already exist.
- `--help`: Print the built-in help text.

## Examples

```lua
touch note.txt
```
Create `note.txt` if needed, or update its modification time if it already exists.

```lua
touch -c cache.lock
```
Only update `cache.lock` if it already exists.

## Notes

- Directory operands are ignored with a warning message instead of being modified.
- When `--no-create` is used for a missing path, the command reports a permission-style failure for that operand.
