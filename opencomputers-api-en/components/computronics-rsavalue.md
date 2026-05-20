# computronics-rsavalue

## Summary

`computronics-rsavalue` documents the RSA key-generator handle returned by the advanced Computronics cipher block. This is not a world component you install directly; it is a generated value object returned by:

- `advanced_cipher.createRandomKeySet(...)`
- `advanced_cipher.createKeySet(...)`

You use this handle to check whether key generation is done and to fetch the finished key pair.

## Availability

- Repository: `computronics`
- Value type: RSA key-generation handle
- Typical source: return value from `advanced_cipher`

## Usage

```lua
local component = require("component")
local advanced = component.advanced_cipher
local keygen = advanced.createRandomKeySet(1024)
```

## API

### finished

- Syntax: `keygen.finished()`
- Returns:
  - Success: `boolean`
  - Failure: `nil, reason`
- Purpose: Reports whether RSA key generation has completed.

Possible failure results:

- `nil, "calculation returned no key set"`
- `nil, "an error occured during key generation"`

Example:

```lua
while true do
  local done, reason = keygen.finished()
  if done == nil then
    error(reason)
  end
  if done then
    break
  end
  os.sleep(0.1)
end
```

### getKeys

- Syntax: `keygen.getKeys()`
- Returns:
  - Success: `publicKey, privateKey`
  - Failure: `nil, nil, reason`
- Purpose: Returns the generated key pair when ready.

Behavior:

- If generation is still running, the handle returns:
  - `nil, nil, "key is still being generated"`
- If generation failed, it returns an error reason.

Returned key format:

- `publicKey[1]`: Base64 modulus
- `publicKey[2]`: Base64 public exponent
- `publicKey[3]`: optional `"prime"`
- `privateKey[1]`: Base64 modulus
- `privateKey[2]`: Base64 private exponent
- `privateKey[3]`: optional `"prime"`

Example:

```lua
local publicKey, privateKey, reason = keygen.getKeys()
if not publicKey then
  error(reason)
end

print("Public modulus:", publicKey[1])
```

## Example

```lua
local keygen = component.advanced_cipher.createRandomKeySet(1024)

repeat
  os.sleep(0.1)
until keygen.finished()

local publicKey, privateKey = keygen.getKeys()
local encrypted = component.advanced_cipher.encrypt("hello", publicKey)
local decrypted = component.advanced_cipher.decrypt(encrypted, privateKey)
print(decrypted)
```

## Notes

- This value object is asynchronous by design.
- If you discard the handle before generation finishes, you also lose direct access to the pending result.

## Related

- `advanced_cipher`
