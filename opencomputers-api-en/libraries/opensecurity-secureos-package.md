# package

## Summary

Module loading state, search-path resolution, and delayed full-module loading helpers.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\package.lua`

## Usage

```lua
local package = require("package")
```

## API

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
