# du

## Summary

Summarize file sizes and recursive directory usage.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\du.lua`

## Syntax

```lua
du [OPTIONS] [FILE...]
```

## Purpose

Print the size of each file argument, or recursively compute the total size of each directory tree. Directory traversal ignores the contents of symbolic links and reports links themselves as size zero.

## Parameters

- `FILE...`: Optional files or directories to inspect. Defaults to the current directory when omitted.

## Options

- `-h, --human-readable`: Print sizes using compact units such as `K`, `M`, and `G`.
- `-s, --summarize`: Print only one total line for each command-line argument.
- `--help`: Print the built-in help text.
- `--version`: Print the built-in version banner.

## Examples

```lua
du
```
Recursively print disk usage for the current directory.

```lua
du -h /home
```
Print `/home` usage with human-readable sizes.

```lua
du -s /tmp /var
```
Only print one total line for `/tmp` and one for `/var`.

## Notes

- A missing path aborts the command immediately with an error.
- This implementation accepts only `-h`, `-s`, `--help`, and `--version`; any other option is rejected.
