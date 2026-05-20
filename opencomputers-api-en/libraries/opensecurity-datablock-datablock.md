# datablock

## Summary

OpenSecurity data-block helpers for hashes, Base64, ROT13, DEFLATE, and hexadecimal conversion through the data-block component.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\lib\datablock.lua`

## Usage

```lua
local datablock = require("datablock")
```

## API

### datablock.crc32(data)

- Description: Return the raw binary CRC-32 digest for the supplied data.
- Parameters:
  - `data`

```lua
datablock.crc32(nil)
```

### datablock.decode64(data)

- Description: Decode a Base64-encoded string through the data-block component.
- Parameters:
  - `data`

```lua
datablock.decode64(nil)
```

### datablock.deflate(data)

- Description: Compress one string with the DEFLATE algorithm through the data-block component.
- Parameters:
  - `data`

```lua
datablock.deflate(nil)
```

### datablock.encode64(data)

- Description: Encode one string into Base64 through the data-block component.
- Parameters:
  - `data`

```lua
datablock.encode64(nil)
```

### datablock.fromHex(hex)

- Description: Convert hexadecimal text back into raw binary data.
- Parameters:
  - `hex`

```lua
datablock.fromHex(nil)
```

### datablock.inflate(data)

- Description: Decompress one DEFLATE-compressed string through the data-block component.
- Parameters:
  - `data`

```lua
datablock.inflate(nil)
```

### datablock.md5(data)

- Description: Return the raw binary MD5 digest for the supplied data.
- Parameters:
  - `data`

```lua
datablock.md5(nil)
```

### datablock.rot13(data)

- Description: Apply the ROT13 substitution transform to one string.
- Parameters:
  - `data`

```lua
datablock.rot13(nil)
```

### datablock.sha256(data)

- Description: Return the raw binary SHA-256 digest for the supplied data.
- Parameters:
  - `data`

```lua
datablock.sha256(nil)
```

### datablock.toHex(data)

- Description: Convert raw binary data into uppercase hexadecimal text.
- Parameters:
  - `data`

```lua
datablock.toHex(nil)
```

## Notes

- Only exported module functions are listed on this page.
