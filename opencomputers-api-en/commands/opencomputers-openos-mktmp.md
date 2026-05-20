# mktmp

## Summary

Create a temporary file or directory with a random name.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mktmp.lua`

## Syntax

```lua
mktmp [OPTIONS] [PATH]
```

## Purpose

Generate a path with `os.tmpname()` and create either an empty file or a directory at that location. By default the result is created under `$TMPDIR`, and the path is printed when stdout is a TTY.

## Parameters

- `PATH`: Optional directory prefix to create the temporary entry under. If omitted, use `$TMPDIR/`.

## Options

- `-d`: Create a directory instead of a file.
- `-v, --verbose`: Always print the resulting path.
- `-q, --quiet`: Suppress printing the resulting path unless `-v` is also used.
- `--help`: Print the built-in help text.

## Examples

```lua
mktmp
```
Create a temporary file below `$TMPDIR`.

```lua
mktmp -d -v /tmp
```
Create a temporary directory under `/tmp` and print the resulting path.

## Notes

- The implementation delegates creation to the regular `touch` and `mkdir` commands resolved through the shell.
- If the chosen prefix path does not already exist, creation fails before `os.tmpname()` output is reported.
