# mount.msdos

## Summary

Mount a DOS FAT filesystem image through the Plan9k `msdosfs` driver.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\mount.msdos.lua`

## Syntax

```lua
mount.msdos
```
```lua
mount.msdos BLOCKDEVICE PATH
```

## Purpose

Without arguments, the command lists existing mounts. With a block-device file path and a mount point, it creates an `msdosfs` proxy and mounts that proxy into the filesystem tree.

## Parameters

- `BLOCKDEVICE`: Path to the backing file that contains the FAT-formatted disk image.
- `PATH`: Directory where the FAT filesystem should be mounted.

## Examples

```lua
mount.msdos /tmp/floppy.img /mnt/floppy
```
Mount the FAT image stored in `/tmp/floppy.img` at `/mnt/floppy`.

## Notes

- The implementation instantiates the driver with FAT type `12`, so it is primarily intended for images created in the same Plan9k toolchain, such as those formatted by `mkdosfs`.
- Note that the addresses may be abbreviated.
