# tree

## Summary

Display directory trees with optional metadata, sorting, colors, and depth limits.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\tree.lua`

## Syntax

```lua
tree [OPTIONS] [FILE...]
```

## Purpose

Walk one or more roots and print each entry with Unicode tree branches. The command supports long-format metadata, file-size formatting, depth limits, full-path output, quoting, coloring, and optional summary counts.

## Parameters

- `FILE...`: Optional root files or directories to print. Defaults to the current directory.

## Options

- `-a, --all`: Include names starting with `.`.
- `--full-time`: With `-l`, print timestamps in full date-time form.
- `-h, --human-readable`: With `-l`, print human-readable sizes using powers of 1024.
- `--si`: With size formatting, use powers of 1000 instead of 1024.
- `--level=LEVEL`: Descend at most `LEVEL` directory levels below each root.
- `--color=auto|always|never`: Control ANSI color output.
- `-l`: Show long-format metadata including type, write flag, size, and time.
- `-f`: Print the full path for each entry instead of only the base name.
- `-i`: Suppress indentation guide lines.
- `-p`: Append `/` to directory names.
- `-Q, --quote`: Wrap each displayed name in double quotes.
- `-r, --reverse`: Reverse the final sorted order at each directory level.
- `-S`: Sort by file size.
- `-t`: Sort by modification time.
- `-X`: Sort by file extension.
- `-C`: Do not print the final file and directory count summary.
- `-R`: Include each root directory itself in the final directory/file counters.
- `--help`: Print the built-in help text.

## Examples

```lua
tree
```
Print the current directory as a tree.

```lua
tree --level=2 -a /home
```
Print `/home` to a maximum depth of two directories, including hidden entries.

```lua
tree -l -h -p /bin
```
Show `/bin` as a tree with long metadata, human-readable sizes, and `/` suffixes for directories.

## Notes

- The traversal yields periodically after roughly two seconds so large trees remain interruptible.
- Sorting order is ascending before `-r` reverses it at each directory level.
