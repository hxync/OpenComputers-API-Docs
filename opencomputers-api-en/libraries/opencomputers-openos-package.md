# package

## Summary

Module loading state, search-path resolution, and delayed full-module loading helpers.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\package.lua`

## Usage

```lua
local package = require("package")
```

## API

### package.delay(lib, file)

- Description: Attach a lazy-loading metatable so a larger module file is only executed on first access.
- Parameters:
  - `lib`
  - `file`

```lua
package.delay(nil, nil)
```

### package.searchpath(name, path, sep, rep)

- Description: Resolve a module name against a semicolon-separated Lua package path template list.
- Parameters:
  - `name`
  - `path`
  - `sep`
  - `rep`

```lua
package.searchpath("value", nil, nil, nil)
```

## Notes

- Only exported module functions are listed on this page.
