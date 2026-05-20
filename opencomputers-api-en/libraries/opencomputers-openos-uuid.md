# uuid

## Summary

Small UUID-v4 style generator that formats random bytes into canonical hexadecimal groups.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\uuid.lua`

## Usage

```lua
local uuid = require("uuid")
```

## API

### uuid.next()

- Description: Generate a random RFC 4122 version-4 style UUID string such as `3c44c8a9-0613-46a2-ad33-97b6ba2e9d9a`.

```lua
local id = uuid.next()
```

## Notes

- Only exported module functions are listed on this page.
