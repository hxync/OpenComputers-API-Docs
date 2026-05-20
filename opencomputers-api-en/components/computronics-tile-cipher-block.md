# cipher

## Summary

The `cipher` component is the basic Computronics encryption block. It derives an AES key and initialization vector from the six inventory slots inside the block, then uses that derived state to encrypt or decrypt data.

This block is useful when you want physical items to define the encryption key. Changing the inserted items changes the resulting cipher state.

## Availability

- Repository: `computronics`
- Component name: `cipher`
- Typical host: placed Computronics cipher block

## Usage

```lua
local component = require("component")
local cipher = component.cipher

if not cipher then
  error("cipher component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Key Model

- The block uses its six inventory slots as physical key material.
- Item id, stack size, damage value, and NBT all influence the derived key.
- The final cipher is `AES/CBC/PKCS5Padding`.
- The block derives:
  - a 16-byte AES key
  - a 16-byte initialization vector

If you change any slot contents, the derived key changes immediately.

## Locking

The block can be locked. While locked:

- inventory slots cannot be read through normal inventory access
- items cannot be inserted or extracted
- sided inventory access reports no accessible slots

The lock state is also persisted in NBT when the configuration allows cipher locks.

## API

### encrypt

- Syntax: `cipher.encrypt(message)`
- Returns: `string`
- Purpose: Encrypts data using the block's current derived AES key.

Parameters:

- `message:string|byte[]`: plaintext to encrypt.

Behavior:

- When a Lua string is supplied, it is encoded as UTF-8 first.
- The result is returned as a Base64 string.
- If the Java cipher instance failed to initialize, the implementation returns an empty string.

Example:

```lua
local encoded = cipher.encrypt("secret message")
print(encoded)
```

### decrypt

- Syntax: `cipher.decrypt(message)`
- Returns: `string`
- Purpose: Decrypts a Base64-encoded ciphertext using the current derived AES key.

Parameters:

- `message:string`: Base64-encoded ciphertext previously produced by `cipher.encrypt`.

Behavior:

- The decrypted bytes are returned as a UTF-8 string.
- Passing data encrypted with a different key layout will usually raise a decryption error.

Example:

```lua
local encoded = cipher.encrypt("hello")
local decoded = cipher.decrypt(encoded)
print(decoded)
```

### isLocked

- Syntax: `cipher.isLocked()`
- Returns: `boolean`
- Purpose: Returns whether the block is currently locked.

Example:

```lua
print("Locked:", cipher.isLocked())
```

### setLocked

- Syntax: `cipher.setLocked(locked)`
- Returns: no meaningful value
- Purpose: Enables or disables the block's lock state.

Parameters:

- `locked:boolean`: `true` to lock the block, `false` to unlock it.

In practice this callback should be treated as a setter without a useful return value.

Example:

```lua
cipher.setLocked(true)
```

## Example

```lua
local plaintext = "door access token"
local ciphertext = cipher.encrypt(plaintext)

print("Encrypted:", ciphertext)
print("Decrypted:", cipher.decrypt(ciphertext))
```

## Notes

- Because the key is derived from the inventory, moving or replacing inserted items can make old ciphertext impossible to decrypt.
- This is the simple symmetric cipher block. For explicit RSA key handling, use the advanced cipher block instead.

## Related

- `component`
- `advanced_cipher`
