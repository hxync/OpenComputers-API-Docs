# factorization

## Summary

This page documents the OpenComputers bridge for Factorization charge conductors.

## Availability

- Dependency: `factorization`
- Label: `integration-required`

## Component Name

- `charge_conductor`

## What This Integration Is Good For

- Monitoring conductor charge in servo or routing systems.
- Detecting whether a conductor line is energized.
- Logging charge changes for automation diagnostics.

## Component

### charge_conductor

`charge_conductor` is exposed on blocks that implement Factorization's `IChargeConductor`.

#### `getCharge()`

- Syntax: `getCharge(): number`
- Purpose: Return the current charge value of the block.
- Returns:
  - The current charge value.
- Usage notes:
  - If the underlying block reports no charge object, the callback returns `0`.
  - The exact meaning of the charge scale depends on the Factorization block you are querying, so compare values from the same machine family rather than assuming a universal range.

#### Example

```lua
local component = require("component")
local conductor = component.charge_conductor

local charge = conductor.getCharge()
print("Charge:", charge)

if charge <= 0 then
  print("Conductor is not carrying charge.")
end
```

## Related

- `redlogic`
