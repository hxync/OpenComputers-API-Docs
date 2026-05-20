# msdos

## Summary

Plan9k FAT12/FAT16 filesystem driver helpers that expose a mounted floppy or image file through a filesystem proxy.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\msdosfs.lua`

## Usage

```lua
local msdos = dofile("/lib/msdosfs.lua")
```

## API

### msdos.proxy(fatfile, fatsize)

- Description: Read the FAT boot sector from a block-like file handle, detect the FAT12 or FAT16 layout, and return a filesystem-style proxy for browsing and modifying that image.
- Parameters:
  - `fatfile`: Open file handle positioned on a FAT image or block device.
  - `fatsize`: Optional forced FAT bit width such as `12` or `16` when autodetection is not desired.

```lua
local proxy = msdos.proxy(io.open('/dev/fd0', 'rb'))
```

## Notes

- The public entry point is `msdos.proxy`; the many `_msdos.*` helpers in the source file are internal parsing and FAT-table utilities.
