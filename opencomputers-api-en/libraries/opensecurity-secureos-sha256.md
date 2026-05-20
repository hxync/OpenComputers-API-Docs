# sha

## Summary

SecureOS SHA-256 helper that falls back to Lua code or delegates to the primary data card when available.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\sha256.lua`

## Usage

```lua
local sha = require("sha256")
```

## API

### sha.sha256(msg)

- Description: Return the hexadecimal SHA-256 digest for one binary string, using the data card implementation when available.
- Parameters:
  - `msg`: Binary string to hash.

```lua
local digest = sha.sha256('hello')
```

## Notes

- Only exported module functions are listed on this page.
