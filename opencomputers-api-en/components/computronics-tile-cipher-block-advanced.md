# advanced_cipher

## Summary

The `advanced_cipher` component is Computronics' RSA-capable encryption block. It can generate RSA key pairs, encrypt data with a public key, decrypt data with a private key, and return asynchronous key-generation handles.

This is the block to use when you need explicit key exchange rather than inventory-derived symmetric encryption.

## Availability

- Repository: `computronics`
- Component name: `advanced_cipher`
- Typical host: placed Computronics advanced cipher block

## Usage

```lua
local component = require("component")
local advanced = component.advanced_cipher

if not advanced then
  error("advanced_cipher component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Key Table Format

RSA keys are represented as Lua tables with integer keys:

- `key[1]`: Base64-encoded modulus `n`
- `key[2]`: Base64-encoded exponent
- `key[3]`: optional string `"prime"`

Two key styles exist:

- normal RSA keys generated from Java key pairs
- prime-generated keys, marked with `key[3] == "prime"`

The advanced cipher accepts either integer keys `1`, `2`, `3` or numeric Lua-table keys `1.0`, `2.0`, `3.0`.

## Energy Model

- Internal energy storage defaults to `1600`
- Key generation consumes the configured key-generation cost
- Encryption and decryption consume:
  - base work cost
  - plus `0.2 * messageLength`

If energy is insufficient, the OC callback returns `nil, nil, reason`.

## API

### createRandomKeySet

- Syntax: `advanced.createRandomKeySet([keyLength])`
- Returns: `keygen`
- Purpose: Starts asynchronous RSA key generation and returns an RSA generator handle.

Parameters:

- `keyLength:number` optional: requested RSA bit length.

Behavior:

- If omitted, the generator defaults to a `1024`-bit key.
- Valid bit lengths are `1` through `2048`.
- The OC context pauses for `0.5` seconds after starting the task.

Errors:

- Invalid sizes raise `bitlength must be between 1 and 2048`

Example:

```lua
local keygen = advanced.createRandomKeySet(1024)
```

### createKeySet

- Syntax: `advanced.createKeySet(num1, num2)`
- Returns: `keygen`
- Purpose: Starts asynchronous RSA key generation from two explicitly supplied prime numbers.

Parameters:

- `num1:number`: first prime
- `num2:number`: second prime

Behavior:

- Both numbers must be prime.
- The generated keys are marked as prime-mode keys.

Errors:

- Non-prime inputs raise errors such as:
  - `bad argument #0 (prime expected, got X)`
  - `bad argument #1 (prime expected, got X)`

Example:

```lua
local keygen = advanced.createKeySet(61, 53)
```

### encrypt

- Syntax: `advanced.encrypt(message, publicKey)`
- Returns:
  - Success: `ciphertext`
  - Failure: `nil, reason`
- Purpose: Encrypts a byte string using the provided RSA public key.

Parameters:

- `message:string|byte[]`: plaintext
- `publicKey:table`: key table described above

Behavior:

- The message is treated as a byte array by the OC callback.
- The returned ciphertext is Base64-encoded.
- Prime-mode keys use direct modular exponentiation.
- Normal keys use Java RSA encryption.

Common failures:

- `nil, "an error occured during encryption"`
- errors about keys being too small for the message
- `nil, nil, reason` when the component lacks energy

Example:

```lua
local publicKey, privateKey
local keygen = advanced.createRandomKeySet(1024)
while not keygen.finished() do
  os.sleep(0.1)
end
publicKey, privateKey = keygen.getKeys()

local ciphertext = assert(advanced.encrypt("hello", publicKey))
```

### decrypt

- Syntax: `advanced.decrypt(message, privateKey)`
- Returns:
  - Success: `plaintext`
  - Failure: `nil, reason`
- Purpose: Decrypts a Base64-encoded RSA ciphertext using the provided private key.

Parameters:

- `message:string|byte[]`: Base64 ciphertext
- `privateKey:table`: key table described above

Behavior:

- Prime-mode keys use direct modular exponentiation.
- Normal keys use Java RSA decryption.
- The final plaintext is returned as a UTF-8 string.

Common failures:

- `nil, "an error occured during decryption"`
- errors about keys being too small
- `nil, nil, reason` when the component lacks energy

Example:

```lua
local decoded = assert(advanced.decrypt(ciphertext, privateKey))
print(decoded)
```

## Keygen Handle

Both `createRandomKeySet` and `createKeySet` return a userdata-like RSA generator object. That object exposes:

- `finished()`
- `getKeys()`

See the separate `computronics-rsavalue` page for the full handle behavior.

## Related

- `component`
- `cipher`
- `computronics-rsavalue`
