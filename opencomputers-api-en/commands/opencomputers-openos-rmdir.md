# rmdir

## Summary

Remove empty directories and optionally their empty parents.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\rmdir.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rmdir`

## Syntax

```lua
rmdir [OPTIONS] DIRECTORY...
```

## Purpose

Remove each directory argument only if it is empty. With `-p`, the command keeps walking upward and removes empty ancestor directories in the same path chain.

## Parameters

- `DIRECTORY...`: One or more directories that must already be empty to be removed.

## Options

- `-q, --ignore-fail-on-non-empty`: Suppress the non-empty-directory diagnostic, though the exit status still records failure.
- `-p, --parents`: Also remove empty parent directories in the same path chain.
- `-v, --verbose`: Print a diagnostic line for each directory processed.
- `--help`: Print the built-in help text.

## Examples

```lua
rmdir cache
```
Remove `cache` if it is empty.

```lua
rmdir -p a/b/c
```
Remove `a/b/c`, then `a/b`, then `a` if each directory is empty at that point.

## Notes

- The literal path `.` is always rejected as an invalid argument.
- The `-q` option only suppresses the non-empty message; it does not convert that case into success.
