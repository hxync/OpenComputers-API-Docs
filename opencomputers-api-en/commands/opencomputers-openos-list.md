# list

## Summary

Print raw directory entries with minimal overhead.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\list.lua`

## Syntax

```lua
list [PATH]
```

## Purpose

List the immediate contents of one directory without any extra formatting, sorting, colors, or metadata columns. The command is intended as a low-memory alternative to `ls`.

## Parameters

- `PATH`: Optional directory to list. Defaults to `.`.

## Options

- `--help`: Print the built-in help text.

## Examples

```lua
list
```
Print one entry per line from the current directory.

```lua
list /bin
```
Print the raw contents of `/bin`.

## Notes

- Only the first positional argument is used.
- The command requires the target path to resolve to an existing directory before listing starts.
