# disk_drive

## Summary

The `disk_drive` component controls a floppy disk drive. Its API is intentionally small: it tells you whether a disk is inserted, gives you the inserted floppy's internal filesystem address, and can eject the medium back into the world.

The same component API is used by the standalone disk drive block and the rack-mountable floppy drive.

## Availability

- Repository: `opencomputers`
- Component name: `disk_drive`
- Typical hosts: disk drive block, rack-mounted disk drive

## Usage

```lua
local component = require("component")
local drive = component.disk_drive

if not drive then
  error("disk_drive component not available")
end
```

## API

### isEmpty

- Syntax: `drive.isEmpty()`
- Returns: `boolean`
- Purpose: Checks whether the drive currently contains a floppy disk.

Return value details:

- `true` means the drive is empty,
- `false` means a disk is inserted.

Example:

```lua
if drive.isEmpty() then
  print("Insert a floppy first")
else
  print("A floppy is present")
end
```

### eject

- Syntax: `drive.eject([velocity])`
- Returns: `boolean`
- Purpose: Ejects the inserted floppy disk into the world.

Parameter:

- `velocity:number` optional: ejection speed multiplier. Source clamps it to `0..1`.

Return value details:

- `true` when a disk was actually ejected,
- `false` when the drive was already empty.

Example:

```lua
local ok = drive.eject(0.5)
print("Ejected:", ok)
```

### media

- Syntax: `drive.media()`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Returns the component address of the inserted floppy's internal filesystem.

Failure reason from source: `"drive is empty"`.

This is the usual bridge from a disk drive to a normal filesystem component:

```lua
local address, reason = drive.media()
if not address then
  print("No disk:", reason)
else
  local fs = component.proxy(address)
  for name in fs.list("/") do
    print(name)
  end
end
```

## Related

- `filesystem`
- `component`
- `openos`
