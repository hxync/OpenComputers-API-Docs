# bit32

## Summary

Lua 5.2-style 32-bit bitwise helpers exposed for compatibility and low-level data work.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\bit32.lua`

## Usage

```lua
local bit32 = require("bit32")
```

## API

### bit32.arshift(x, disp)

- Description: Arithmetic right-shift a 32-bit integer.
- Parameters:
  - `x`
  - `disp`

```lua
bit32.arshift(nil, nil)
```

### bit32.band(...)

- Description: Bitwise-AND all supplied operands.
- Parameters:
  - `...`

```lua
bit32.band(nil)
```

### bit32.bnot(x)

- Description: Bitwise-NOT a 32-bit integer.
- Parameters:
  - `x`

```lua
bit32.bnot(nil)
```

### bit32.bor(...)

- Description: Bitwise-OR all supplied operands.
- Parameters:
  - `...`

```lua
bit32.bor(nil)
```

### bit32.btest(...)

- Description: Return whether the bitwise-AND of all operands is non-zero.
- Parameters:
  - `...`

```lua
bit32.btest(nil)
```

### bit32.bxor(...)

- Description: Bitwise-XOR all supplied operands.
- Parameters:
  - `...`

```lua
bit32.bxor(nil)
```

### bit32.extract(n, field, width)

- Description: Extract a bit field starting at the given offset and width.
- Parameters:
  - `n`
  - `field`
  - `width`

```lua
bit32.extract(nil, nil, nil)
```

### bit32.lrotate(x, disp)

- Description: Rotate bits to the left within a 32-bit value.
- Parameters:
  - `x`
  - `disp`

```lua
bit32.lrotate(nil, nil)
```

### bit32.lshift(x, disp)

- Description: Logically shift bits to the left.
- Parameters:
  - `x`
  - `disp`

```lua
bit32.lshift(nil, nil)
```

### bit32.replace(n, v, field, width)

- Description: Replace a bit field inside a 32-bit integer with a new value.
- Parameters:
  - `n`
  - `v`
  - `field`
  - `width`

```lua
bit32.replace(nil, nil, nil, nil)
```

### bit32.rrotate(x, disp)

- Description: Rotate bits to the right within a 32-bit value.
- Parameters:
  - `x`
  - `disp`

```lua
bit32.rrotate(nil, nil)
```

### bit32.rshift(x, disp)

- Description: Logically shift bits to the right.
- Parameters:
  - `x`
  - `disp`

```lua
bit32.rshift(nil, nil)
```

## Notes

- Only exported module functions are listed on this page.
