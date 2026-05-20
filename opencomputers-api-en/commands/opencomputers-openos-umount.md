# umount

## Summary

Remove a mounted filesystem by mount path or by filesystem identity.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\umount.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\umount`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\umount`

## Syntax

```lua
umount MOUNT
```
```lua
umount -a FILESYSTEM
```

## Purpose

Without `-a`, the operand must resolve to an exact mount point path. With `-a`, the operand is treated as a filesystem label or address, and abbreviated addresses are accepted as long as they resolve uniquely.

## Parameters

- `MOUNT`: Mount point path to detach when operating in path mode.
- `FILESYSTEM`: Filesystem label or address used with `-a`.

## Options

- `-a`: Unmount by filesystem label or address instead of by exact mount path.

## Examples

```lua
umount /mnt/disk
```
Unmount the filesystem mounted exactly at `/mnt/disk`.

```lua
umount -a e4f3
```
Unmount the filesystem whose label or abbreviated address resolves to `e4f3`.

## Notes

- Path mode rejects paths that merely lie inside a mount; the path itself must be the mount point.
- If no matching mount is found, the command prints `nothing to unmount here`.
