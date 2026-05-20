# procfs

## Summary

Internal pseudo-file handle helpers used by Plan9k's `/proc` filesystem implementation.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\10_procfs.lua`

## Usage

```lua
-- internal procfs handle helper mounted under /proc
```

## API

### procfs.read(n)

- Description: Read a slice of the synthetic procfs file data associated with the current open handle.
- Parameters:
  - `n`: Optional maximum number of bytes to read from the current handle position.

```lua
local chunk = procfs.read(128)
```

## Notes

- Only exported module functions are listed on this page.
