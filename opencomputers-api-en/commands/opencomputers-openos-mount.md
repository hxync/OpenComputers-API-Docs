# mount

## Summary

List current mounts, or attach a filesystem into the OpenOS directory tree.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mount.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mount`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mount`

## Syntax

```lua
mount
```
```lua
mount LABEL PATH
```
```lua
mount ADDRESS PATH
```
```lua
mount --bind PATH PATH
```

## Purpose

Without arguments, print the currently mounted filesystems. With a device and target path, mount that filesystem into the unified OpenOS tree, or create a bind mount when `--bind` is used.

## Parameters

- `LABEL`: Filesystem label to mount.
- `ADDRESS`: Filesystem address prefix to mount.
- `PATH`: Mount point path in the OpenOS tree.

## Options

- `-r, --readonly`: Mount the filesystem as read-only.
- `--bind`: Create a bind mount from one folder path to another.
- `-h, --help`: Print the usage help message.

## Examples

```lua
mount
```
Display all current mount points.

```lua
mount test /home
```
Mount the filesystem labeled `test` at `/home`.

```lua
mount 56f /var
```
Mount the filesystem whose address starts with `56f` at `/var`.

```lua
mount --readonly tmpfs /tmp_ro
```
Expose `tmpfs` at `/tmp_ro` in read-only mode.

```lua
mount --bind /mnt/fa4/home /home
```
Bind `/mnt/fa4/home` onto `/home`.

## Notes

- The source device may be selected by label, address prefix, or folder path when `--bind` is used.
- If options are supplied without the required positional arguments, the command prints usage and exits with an error.
