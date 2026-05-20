# ec

## Summary

This page documents the Extra Cells integration exposed to Lua by OpenComputers. It extends AE network-facing components with gas listing support.

## Availability

- Dependency: `ec`
- Label: `integration-required`

## Component Names

- `me_controller`
- `me_interface`

## What This Integration Is Good For

- Listing stored gases available through an Extra Cells AE network.
- Extending existing AE controller and interface automation with gas awareness.

## Components

### me_controller

When the `ec` integration is available, `me_controller` gains one additional gas-related callback.

#### `getGasesInNetwork()`

- Syntax: `getGasesInNetwork(): table`
- Purpose: Return all gas stacks currently stored in the AE network.
- Returns:
  - A Lua array of gas stack descriptor tables.
- Usage notes:
  - Only fluid entries recognized by Extra Cells as gases are returned.
  - This supplements the normal `appeng` network inspection callbacks.

### me_interface

Block `me_interface` components also gain the same gas callback when backed by an Extra Cells-capable network.

#### `getGasesInNetwork()`

- Syntax: `getGasesInNetwork(): table`
- Purpose: Return all gas stacks currently visible in the connected AE network.
- Returns:
  - A Lua array of gas stack descriptor tables.
- Usage notes:
  - This is most useful when your automation is already centered on interface blocks instead of controllers.

## Example

```lua
local component = require("component")
local me = component.me_controller

for _, gas in ipairs(me.getGasesInNetwork()) do
  print(gas.label or gas.name, gas.amount)
end
```

## Related

- `appeng`
- `mekanism`
