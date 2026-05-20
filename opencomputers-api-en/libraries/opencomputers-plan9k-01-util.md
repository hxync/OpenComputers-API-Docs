# allocator

## Summary

Internal ID allocator helpers used by several Plan9k kernel-style modules.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\01_util.lua`

## Usage

```lua
-- internal Plan9k utility allocator returned by getAllocator()
```

## API

### allocator.get()

- Description: Allocate the next free numeric entry and return the reusable table stored for that slot.

```lua
local entry = allocator:get()
```

### allocator.unset(e)

- Description: Release one previously allocated entry back into the allocator's free list.
- Parameters:
  - `e`: Allocated entry object previously returned by `allocator:get()`.

```lua
allocator:unset(entry)
```

## Notes

- Only exported module functions are listed on this page.
