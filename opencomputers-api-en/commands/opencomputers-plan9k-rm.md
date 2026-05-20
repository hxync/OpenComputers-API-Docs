# rm

## Summary

Remove files, symbolic links, or directories.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\rm.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rm`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\rm`

## Syntax

```lua
rm [OPTIONS] FILE...
```

## Purpose

Remove the specified paths one by one. Regular files and links can be deleted directly, empty directories require `-d`, and non-empty directories require recursive removal.

## Parameters

- `FILE...`: One or more files, links, or directories to remove.

## Options

- `-f, --force`: Ignore missing files, suppress prompts, and suppress normal status output.
- `-i`: Prompt before every removal attempt.
- `-I`: Prompt once before removing more than three arguments.
- `-r, -R, --recursive`: Recursively remove directory contents.
- `-d, --dir`: Allow removal of empty directories without recursive mode.
- `-v, --verbose`: Print a message for each successfully removed path.
- `--help`: Print the built-in usage help text.

## Examples

```lua
rm a.txt
```
Delete the file `a.txt`.

```lua
rm -d empty_dir
```
Remove `empty_dir` if it is empty.

```lua
rm -r cache
```
Recursively remove the `cache` directory and everything inside it.

```lua
rm -iv a.txt b.txt
```
Prompt before each removal and print each successful deletion.

## Notes

- Read-only targets are rejected even in recursive mode.
- Use `umount` for mounted filesystems instead of trying to delete the mount path with `rm`.
