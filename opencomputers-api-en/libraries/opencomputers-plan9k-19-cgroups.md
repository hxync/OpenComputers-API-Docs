# cgroups

## Summary

Plan9k cgroup-constructor helpers used to sandbox component visibility and module loading state.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\19_cgroups.lua`

## Usage

```lua
-- internal cgroup constructors exposed to Plan9k userspace helpers
```

## API

### cgroups.component(wl, bl)

- Description: Create a component-visibility group that inherits from the current thread and applies whitelist or blacklist filtering.
- Parameters:
  - `wl`: Optional whitelist table keyed by component address.
  - `bl`: Optional blacklist table keyed by component address.

```lua
local group = cgroups.component(whitelist, blacklist)
```

### cgroups.module()

- Description: Create a fresh module-loading group with its own preload table, loaded set, loading set, and searcher list.

```lua
local group = cgroups.module()
```

## Notes

- Only exported module functions are listed on this page.
