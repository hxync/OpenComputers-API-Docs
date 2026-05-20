# mkdosfs

## Summary

Create a small FAT12 or FAT16 filesystem inside a block-device file.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\mkdosfs.lua`

## Syntax

```lua
mkdosfs IMAGE_FILE
```

## Purpose

Open the target file in binary write mode, treat it as a 512-byte-sector block device, calculate FAT layout values from its size, write a boot record, initialize both FAT tables, and clear the root directory.

## Parameters

- `IMAGE_FILE`: Existing writable file that will be overwritten with a FAT filesystem layout.

## Examples

```lua
mkdosfs /tmp/floppy.img
```
Format `/tmp/floppy.img` as a DOS filesystem image.

## Notes

- The command does not create the file for you; it expects an already existing writable image file.
- Everything currently stored in the target file is destroyed.
