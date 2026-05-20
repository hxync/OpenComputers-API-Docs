# opencomputers-data-card

## Summary

This filename is legacy. The real Lua component name is `data`.

The data card provides hashing, encoding, compression, encryption, random generation, and elliptic-curve key operations. Available callbacks depend on the card tier.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `data`
- Backing implementation: `li.cil.oc.server.component.DataCard`
- Typical hosts: machines or devices fitted with a data card

## Tier Breakdown

### Tier 1

- `getLimit`
- `encode64`
- `decode64`
- `deflate`
- `inflate`
- `crc32`
- `md5`
- `sha256`
- `decodeNBT`

### Tier 2

Adds:

- HMAC support for `md5`
- HMAC support for `sha256`
- `encrypt`
- `decrypt`
- `random`

### Tier 3

Adds:

- `generateKeyPair`
- `deserializeKey`
- `ecdh`
- `ecdsa`

## Important Rules

### Binary Strings

Many callbacks return raw binary data, not printable text.

Typical examples:

- hashes
- AES ciphertext
- random bytes
- serialized keys
- ECDSA signatures

If you need text-safe transport or storage, encode the result with `encode64()`.

### Data Size Limit

`getLimit()` returns the hard maximum input size accepted by the card's data-processing callbacks.

- If the first binary/string argument exceeds that limit, the callback throws `data size limit exceeded`.
- Large inputs above the soft limit also make the computer pause for a configured timeout.

### Energy Use

All callbacks consume connector energy.

- If the host has insufficient energy buffer, the callback fails with `not enough energy`.

## data API

### getLimit

- Syntax: `data.getLimit()`
- Returns: `number`
- Purpose: Returns the hard maximum number of bytes accepted by the card's byte-processing callbacks.

### encode64

- Syntax: `data.encode64(bytes)`
- Parameters:
  - `bytes:string`: raw binary or text data.
- Returns: `string`
- Purpose: Encodes the input using Base64.

### decode64

- Syntax: `data.decode64(text)`
- Parameters:
  - `text:string`: Base64 text.
- Returns: `string`
- Purpose: Decodes Base64 input back into raw bytes.

### deflate

- Syntax: `data.deflate(bytes)`
- Parameters:
  - `bytes:string`: raw input data.
- Returns: `string`
- Purpose: Compresses the input using the deflate format.

### inflate

- Syntax: `data.inflate(bytes)`
- Parameters:
  - `bytes:string`: deflated binary data.
- Returns: `string`
- Purpose: Decompresses deflated binary data.

### crc32

- Syntax: `data.crc32(bytes)`
- Parameters:
  - `bytes:string`
- Returns: `string`
- Purpose: Computes the CRC-32 digest of the input.

The result is a 4-byte binary digest, not hexadecimal text.

### md5

- Syntax:
  - `data.md5(bytes)`
  - `data.md5(bytes, hmacKey)`
- Parameters:
  - `bytes:string`
  - `hmacKey:string`: optional HMAC key, Tier 2+ only.
- Returns: `string`
- Purpose: Computes either an MD5 digest or an HMAC-MD5 digest.

Notes:

- Plain MD5 exists on Tier 1.
- The HMAC form requires Tier 2 or higher.
- The result is binary data.

### sha256

- Syntax:
  - `data.sha256(bytes)`
  - `data.sha256(bytes, hmacKey)`
- Parameters:
  - `bytes:string`
  - `hmacKey:string`: optional HMAC key, Tier 2+ only.
- Returns: `string`
- Purpose: Computes either a SHA-256 digest or an HMAC-SHA256 digest.

The result is binary data.

### decodeNBT

- Syntax: `data.decodeNBT(bytes)`
- Parameters:
  - `bytes:string`: gzipped binary NBT payload.
- Returns: `table`
- Purpose: Decodes compressed binary NBT data into a Lua table representation.

This is useful for inspecting serialized Minecraft data blobs.

### encrypt

- Syntax: `data.encrypt(bytes, key, iv)`
- Parameters:
  - `bytes:string`: plaintext data.
  - `key:string`: exactly 16 bytes.
  - `iv:string`: exactly 16 bytes.
- Returns: `string`
- Purpose: Encrypts data with `AES/CBC/PKCS5Padding`.

Failure cases:

- wrong key length: `expected a 128-bit AES key`
- wrong IV length: `expected a 128-bit AES IV`

The result is binary ciphertext.

### decrypt

- Syntax: `data.decrypt(bytes, key, iv)`
- Parameters:
  - `bytes:string`: ciphertext.
  - `key:string`: exactly 16 bytes.
  - `iv:string`: exactly 16 bytes.
- Returns: `string`
- Purpose: Decrypts `AES/CBC/PKCS5Padding` ciphertext.

The returned plaintext is raw bytes.

### random

- Syntax: `data.random(length)`
- Parameters:
  - `length:number`: number of random bytes to generate.
- Returns: `string`
- Purpose: Returns cryptographically secure random bytes.

Valid range:

- `1` to `1024`

Invalid lengths throw:

- `length must be in range [1..1024]`

### generateKeyPair

- Syntax: `data.generateKeyPair([bitLen])`
- Parameters:
  - `bitLen:number`: optional curve size, `256` or `384`. Defaults to `384`.
- Returns: `userdata, userdata`
- Purpose: Generates an EC public/private key pair.

Return order:

- first value: public key userdata
- second value: private key userdata

Invalid sizes throw:

- `invalid key length, must be 256 or 384`

### deserializeKey

- Syntax: `data.deserializeKey(bytes, typeName)`
- Parameters:
  - `bytes:string`: serialized key bytes.
  - `typeName:string`: `ec-public` or `ec-private`.
- Returns: `userdata`
- Purpose: Restores an EC key userdata object from serialized bytes.

Invalid type names throw:

- `invalid key type, must be ec-public or ec-private`

### ecdh

- Syntax: `data.ecdh(privateKey, publicKey)`
- Parameters:
  - `privateKey:userdata`: EC private key userdata.
  - `publicKey:userdata`: EC public key userdata.
- Returns: `string`
- Purpose: Derives a shared secret using ECDH.

The result is raw binary secret data.

Argument mismatches throw errors such as:

- `private key expected at 1`
- `public key expected at 2`

### ecdsa

- Syntax:
  - `data.ecdsa(bytes, privateKey)`
  - `data.ecdsa(bytes, publicKey, signature)`
- Parameters:
  - `bytes:string`: message bytes.
  - `privateKey:userdata`: for signing mode.
  - `publicKey:userdata`: for verify mode.
  - `signature:string`: binary signature to verify.
- Returns:
  - Signing mode: `string`
  - Verify mode: `boolean`
- Purpose: Signs or verifies data with `SHA256withECDSA`.

Behavior:

- With no signature argument, the function signs and returns a binary signature.
- With a signature argument, the function verifies and returns `true` or `false`.

## EC Key Userdata API

The objects returned by `generateKeyPair()` and `deserializeKey()` expose their own methods.

### isPublic

- Syntax: `key.isPublic()`
- Returns: `boolean`
- Purpose: Returns whether the key userdata wraps a public key.

### keyType

- Syntax: `key.keyType()`
- Returns: `string`
- Purpose: Returns `ec-public` or `ec-private`.

### serialize

- Syntax: `key.serialize()`
- Returns: `string`
- Purpose: Returns the encoded binary form of the key.

Use `data.encode64()` if you need to store or print it safely.

## Example

```lua
local component = require("component")
local data = component.data

local digest = data.sha256("hello world")
print("sha256(base64):", data.encode64(digest))

local publicKey, privateKey = data.generateKeyPair(256)
local signature = data.ecdsa("message", privateKey)
print("signature ok:", data.ecdsa("message", publicKey, signature))

local secret = data.random(16)
local iv = data.random(16)
local cipher = data.encrypt("secret text", secret, iv)
print("cipher(base64):", data.encode64(cipher))
print("plain:", data.decrypt(cipher, secret, iv))
```

## Related

- `database`
- `internet`
