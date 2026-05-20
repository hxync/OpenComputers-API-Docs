# libarmor

## Summary

Protect a table behind a proxy that preserves reads and iteration while denying unauthorized writes.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\libarmor.lua`

## Usage

```lua
local libarmor = require("libarmor")
```

## API

### libarmor.protect(original, whitelist)

- Description: Return a protected proxy around one original table, allowing writes only for keys present in the optional whitelist.
- Parameters:
  - `original`: Original table whose readable contents should be exposed through the proxy.
  - `whitelist`: Optional table of keys that are allowed to be reassigned through the proxy.

```lua
local safe = libarmor.protect(config, {volume=true})
```

## Notes

- Only exported module functions are listed on this page.
