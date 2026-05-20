# debug

## Summary

Small SecureOS toy and timing helpers for benchmarking loops, coin tosses, and dice rolls.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\debug.lua`

## Usage

```lua
local debug = require("debug")
```

## API

### debug.benchmark(amt)

- Description: Run a simple summation loop and return the elapsed CPU time as a formatted string.
- Parameters:
  - `amt`

```lua
debug.benchmark(nil)
```

### debug.coinToss(times)

- Description: Simulate repeated coin flips and return the final number of heads and tails.
- Parameters:
  - `times`

```lua
debug.coinToss(nil)
```

### debug.diceRoll(amt,sides,mod)

- Description: Return the total for a simple dice expression using one random roll, an amount multiplier, and an optional modifier.
- Parameters:
  - `amt`
  - `sides`
  - `mod`

```lua
debug.diceRoll(nil, nil, nil)
```

## Notes

- Only exported module functions are listed on this page.
