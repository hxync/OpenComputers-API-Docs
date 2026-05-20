# drive

## Summary

The `drive` component exposes low-level sector and byte access to OpenComputers hard drives and similar block devices. It is much lower level than the normal `filesystem` component: instead of files and directories, you work with raw bytes and sectors.

This API is mainly useful for custom formats, boot loaders, disk inspection, forensic tooling, and storage experiments.

## Availability

- Repository: `opencomputers`
- Component name: `drive`
- Typical hosts: hard drives and compatible raw block devices

## Important Notes

- Lua-facing sector numbers and byte offsets are `1`-based.
- Sectors are always `512` bytes.
- Read and write calls consume call budget and may pause when the virtual drive head has to seek.
- Some drives are read-only and will reject write operations.

## Usage

```lua
local component = require("component")
local drive = component.drive

if not drive then
  error("drive component not available")
end
```

## API

### getLabel

- Syntax: `drive.getLabel()`
- Returns: `string` or `nil`
- Purpose: Reads the current drive label.

Some drive implementations may not support labeling, in which case this can be `nil`.

### setLabel

- Syntax: `drive.setLabel(value)`
- Returns: `string`
- Purpose: Changes the drive label and returns the resulting label.

Parameter:

- `value:string`: desired label text.

Possible errors from source:

- `"drive is read only"`
- `"drive does not support labeling"`

### getCapacity

- Syntax: `drive.getCapacity()`
- Returns: `number`
- Purpose: Gets the total raw drive capacity in bytes.

### getSectorSize

- Syntax: `drive.getSectorSize()`
- Returns: `number`
- Purpose: Gets the sector size in bytes.

This is fixed at `512`.

### getPlatterCount

- Syntax: `drive.getPlatterCount()`
- Returns: `number`
- Purpose: Gets the number of platters emulated by the drive.

This mostly matters for seek behavior and low-level simulation, not for normal file storage.

### readSector

- Syntax: `drive.readSector(sector)`
- Returns: `string`
- Purpose: Reads one full sector.

Parameter:

- `sector:number`: sector number, starting at `1`.

Behavior:

- The return value is a `512`-byte Lua string.
- Invalid sector numbers raise `"invalid offset, not in a usable sector"`.

Example:

```lua
local raw = drive.readSector(1)
print("Bytes returned:", #raw)
```

### writeSector

- Syntax: `drive.writeSector(sector, value)`
- Returns: no direct return values
- Purpose: Writes raw data into one sector.

Parameters:

- `sector:number`: sector number, starting at `1`.
- `value:string`: raw bytes to write.

Behavior:

- The drive must not be read-only.
- If `value` is shorter than 512 bytes, only the provided prefix is overwritten; the remaining bytes in that sector stay unchanged.
- Invalid sector numbers raise `"invalid offset, not in a usable sector"`.

Possible error from source: `"drive is read only"`.

### readByte

- Syntax: `drive.readByte(offset)`
- Returns: `number`
- Purpose: Reads one byte from a raw byte offset.

Parameter:

- `offset:number`: byte offset, starting at `1`.

Important detail:

- Source stores bytes in a signed Java byte array, so the returned number may appear in the `-128..127` range.
- If you want an unsigned value, normalize it in Lua:

```lua
local b = drive.readByte(1)
local unsigned = (b + 256) % 256
```

### writeByte

- Syntax: `drive.writeByte(offset, value)`
- Returns: no direct return values
- Purpose: Writes one byte to a raw byte offset.

Parameters:

- `offset:number`: byte offset, starting at `1`.
- `value:number`: byte value to write.

Possible error from source: `"drive is read only"`.

### Raw Access Example

```lua
local sector = drive.readSector(1)
local first = drive.readByte(1)
print("Sector size:", drive.getSectorSize())
print("Signed first byte:", first)
drive.writeByte(1, 0x42)
```

## Related

- `filesystem`
- `eeprom`
- `component`
