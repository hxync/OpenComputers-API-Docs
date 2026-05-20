# find

## Summary

Recursively search for files, directories, or symbolic links.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\find.lua`

## Syntax

```lua
find [PATH] [--type=d|f|s] [--name=EXPR] [--iname=EXPR]
```

## Purpose

Walk the target directory tree and print every path that matches the requested type and optional name expression. Results are printed as relative paths based on the starting path you pass in.

## Parameters

- `PATH`: Optional starting directory. Defaults to the current working directory.
- `EXPR`: Glob-like name expression where `*` matches any sequence of characters.

## Options

- `--type=d`: Only print directories.
- `--type=f`: Only print regular files.
- `--type=s`: Only print symbolic links.
- `--name=EXPR`: Match names against `EXPR` using the implementation's current comparison mode.
- `--iname=EXPR`: Match names against `EXPR` using the implementation's alternate comparison mode.
- `--help`: Print the built-in usage text.

## Examples

```lua
find
```
Recursively list every file-system entry under the current directory.

```lua
find /etc --type=f
```
Only print regular files under `/etc`.

```lua
find /home --name='*.lua'
```
Search `/home` for paths whose final name matches `*.lua`.

## Notes

- The implementation converts `*` into a Lua pattern wildcard and requires the whole file name to match.
- Source behavior currently makes `--name` case-insensitive and `--iname` case-sensitive, which is the opposite of the help text.
