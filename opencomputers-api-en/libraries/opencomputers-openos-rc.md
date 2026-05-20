# rc

## Summary

Service registry and runlevel helper functions for `rc`-managed startup scripts.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\rc.lua`

## Usage

```lua
local rc = require("rc")
```

## API

## Notes

- The small bootstrap module only seeds `rc.loaded`; the remaining helpers are added by the delayed full implementation used by normal OpenOS-style systems.
