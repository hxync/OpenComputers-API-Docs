# mount.cow

## Summary

Mount a copy-on-write overlay built from a read source and a write layer.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\mount.cow.lua`

## Syntax

```lua
mount.cow
```
```lua
mount.cow READ_FS WRITE_FS PATH
```

## Purpose

Without arguments, the command lists current mounts. With three arguments, it resolves two filesystem proxies, wraps them with `pipes.cowProxy`, and mounts the resulting overlay at the target path.

## Parameters

- `READ_FS`: Label or address of the lower, read source filesystem.
- `WRITE_FS`: Label or address of the writable upper filesystem that stores changes.
- `PATH`: Mount point where the merged view should appear.

## Examples

```lua
mount.cow
```
Print the current mount table.

```lua
mount.cow 123 456 /mnt/work
```
Mount a copy-on-write view of filesystem `123` with changes stored on filesystem `456` at `/mnt/work`.

## Notes

- Component addresses may be abbreviated as long as `filesystem.proxy` can resolve them uniquely.
- Note that the addresses may be abbreviated.
