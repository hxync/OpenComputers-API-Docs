# os_datablock

## Summary

The `os_datablock` component is OpenSecurity's utility data block. It offers a group of local data-processing callbacks such as Base64, compression, hashes, ROT13, and BCrypt helpers, and it also mounts a bundled filesystem environment named `datablock`.

## Availability

- Repository: `opensecurity`
- Component name: `os_datablock`
- Typical host: placed OpenSecurity data block

## Usage

```lua
local component = require("component")
local data = component.os_datablock

if not data then
  error("os_datablock component not installed")
end
```

## Filesystem Environment

When connected to a computer context, the block also exposes a bundled filesystem environment named `datablock`.

## Data Limits And Energy

Several byte-array callbacks pass through a shared limiter:

- hard size limit: `Settings.get().dataCardHardLimit()`
- soft size limit: `Settings.get().dataCardSoftLimit()`
- timeout pause for oversized but still allowed data: `Settings.get().dataCardTimeout()`

Shared failure modes for callbacks using that limiter:

- oversized input: `data size limit exceeded`
- insufficient power: `not enough energy`

Energy classes used by the source:

- complex-cost callbacks:
  - `encode64`
  - `decode64`
  - `deflate`
  - `inflate`
- simple-cost callbacks:
  - `sha256`
  - `md5`
  - `crc32`
- no limiter and no explicit power draw:
  - `rot13`
  - `bCryptHash`
  - `bCryptCheck`

## API

### greet

- Syntax: `data.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

### getLimit

- Syntax: `data.getLimit()`
- Returns: `number`
- Purpose: Returns the hard maximum byte size accepted by the limited callbacks.

### encode64

- Syntax: `data.encode64(input)`
- Returns: `string`
- Purpose: Base64-encodes the supplied byte string.

Parameters:

- `input:string|byte[]`

Behavior:

- The return value is a byte string containing ASCII Base64 text.

### decode64

- Syntax: `data.decode64(input)`
- Returns: `string`
- Purpose: Base64-decodes the supplied byte string.

Parameters:

- `input:string|byte[]`

Behavior:

- The return value is the decoded raw byte string.

### deflate

- Syntax: `data.deflate(input)`
- Returns: `string`
- Purpose: Compresses the supplied bytes using deflate.

Parameters:

- `input:string|byte[]`

### inflate

- Syntax: `data.inflate(input)`
- Returns: `string`
- Purpose: Inflates a previously deflated byte string.

Parameters:

- `input:string|byte[]`

### sha256

- Syntax: `data.sha256(input)`
- Returns: `string`
- Purpose: Computes the SHA-256 digest.

Parameters:

- `input:string|byte[]`

Behavior:

- The result is returned in binary format, not hexadecimal text.

### md5

- Syntax: `data.md5(input)`
- Returns: `string`
- Purpose: Computes the MD5 digest.

Parameters:

- `input:string|byte[]`

Behavior:

- The result is returned in binary format.

### crc32

- Syntax: `data.crc32(input)`
- Returns: `string`
- Purpose: Computes the CRC-32 digest.

Parameters:

- `input:string|byte[]`

Behavior:

- The result is returned in binary format.

### rot13

- Syntax: `data.rot13(input)`
- Returns: `string`
- Purpose: Applies ROT13 to alphabetic ASCII characters.

Parameters:

- `input:string`

Behavior:

- Only letters `A-Z` and `a-z` are rotated.
- All other characters are copied unchanged.

### bCryptHash

- Syntax: `data.bCryptHash(plaintext[, rounds])`
- Returns: `string`
- Purpose: Computes a BCrypt hash string.

Parameters:

- `plaintext:string`
- `rounds:number` optional

Behavior:

- Default rounds value is `10`.
- The implementation clamps rounds to `4` through `15`.
- The returned value is the standard BCrypt text hash.

### bCryptCheck

- Syntax: `data.bCryptCheck(plaintext, hash)`
- Returns: `boolean`
- Purpose: Verifies plaintext against a BCrypt hash string.

Parameters:

- `plaintext:string`
- `hash:string`

## Example

```lua
local digest = data.sha256("hello")
local encoded = data.encode64("secret")
local compressed = data.deflate("compress me")
local hash = data.bCryptHash("password", 12)

print(#digest, encoded, data.bCryptCheck("password", hash))
print(data.rot13("uryyb jbeyq"))
```

## Related

- `component`
- `os_cardwriter`
