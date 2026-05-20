# ls

## Summary

List files and directories with optional formatting, sorting, and recursion.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\ls.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\ls`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\ls`

## Syntax

```lua
ls [OPTIONS] [FILE...]
```

## Purpose

Display information about the requested files or directories, or the current working directory when no operand is provided. The command supports long listings, recursive traversal, multiple sort modes, optional colors, and Microsoft-style summary output.

## Parameters

- `FILE...`: Optional files or directories to inspect. If omitted, list the current working directory.

## Options

- `-a, --all`: Include entries whose names start with `.`.
- `--full-time`: With `-l`, print timestamps in full ISO-like date and time format.
- `-h, --human-readable`: With size output, use powers of 1024 and print compact human-readable units.
- `--si`: With size output, use powers of 1000 instead of 1024.
- `-l`: Use long listing format with type, write flag, size, and modification time.
- `-r, --reverse`: Reverse the final display order.
- `-R, --recursive`: Recursively list subdirectories.
- `-S`: Sort by file size, largest first.
- `-t`: Sort by modification time, newest first.
- `-X`: Sort alphabetically by file extension.
- `-1`: Print one entry per line.
- `-p`: Append `/` to displayed directory names.
- `-M`: Print Microsoft-style file and directory counts plus free-space information after each listing.
- `--no-color`: Disable ANSI colorized output even when writing to a terminal.
- `--help`: Print the built-in usage help text.

## Examples

```lua
ls
```
List the current working directory.

```lua
ls -l /bin
```
Show `/bin` in long listing format.

```lua
ls -aR /home
```
Recursively list `/home`, including hidden entries.

```lua
ls -M /mnt
```
List `/mnt` and print DOS-style file and directory totals afterward.

## Notes

- When standard output is a terminal, short listings are arranged into columns unless `-1` or `-l` is used.
- If the full-featured implementation cannot be loaded because the machine is low on memory, the wrapper suggests using `list` instead.
