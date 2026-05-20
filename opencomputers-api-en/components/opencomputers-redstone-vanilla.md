# opencomputers-redstone-vanilla

## Summary

This page documents the standard `redstone` component callbacks for normal analog redstone input and output. They are provided by redstone cards, adapters, relays, switches, microcontrollers with redstone capability, and other hosts that expose OpenComputers redstone control.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `redstone`
- Backing trait: `RedstoneVanilla`

## What It Does

- Reads analog redstone input on one side or all six sides.
- Reads the current analog redstone output values maintained by the component.
- Sets one side or all sides of analog redstone output.
- Reads comparator override strength from adjacent blocks.

Side numbers use the normal OpenComputers side constants `0` through `5`.

## Signals

When a redstone input changes, the component emits:

```lua
"redstone_changed", side, oldValue, newValue
```

- `side` is the side number that changed.
- `oldValue` and `newValue` are analog strengths.

If the component is configured with a wake threshold and the new value crosses that threshold upward, it can also wake sleeping computers.

## API

### getInput

- Syntax: `redstone.getInput([side])`
- Returns:
  - With `side`: `number`
  - Without `side`: `table`
- Purpose: Reads incoming analog redstone strength.

When called without arguments, the returned table is keyed by side number.

### getOutput

- Syntax: `redstone.getOutput([side])`
- Returns:
  - With `side`: `number`
  - Without `side`: `table`
- Purpose: Reads the component's currently configured analog redstone output value.

### setOutput

- Syntax:
  - `redstone.setOutput(side, value)`
  - `redstone.setOutput(valuesTable)`
- Returns:
  - Single-side form: previous output on that side
  - Table form: previous outputs for all sides
- Purpose: Updates analog redstone output.

`valuesTable` should map side numbers to strength values.

### getComparatorInput

- Syntax: `redstone.getComparatorInput(side)`
- Returns: `number`
- Purpose: Reads comparator override strength from the block adjacent to the specified side.

If the adjacent block does not provide comparator output, the function returns `0`.

## Practical Notes

- Analog values normally use the Minecraft redstone range `0` to `15`.
- Calling `setOutput` may pause briefly if the OpenComputers redstone delay setting is enabled.
- Side numbers are interpreted relative to the host's orientation and then converted to world sides internally.

## Example

```lua
local component = require("component")
local sides = require("sides")
local redstone = component.redstone

print("Front input:", redstone.getInput(sides.front))
print("All outputs:", redstone.getOutput())

local previous = redstone.setOutput(sides.back, 15)
print("Previous back output:", previous)
print("Comparator on top:", redstone.getComparatorInput(sides.top))
```

## Related

- `opencomputers-redstone-bundled`
- `opencomputers-redstone-wireless`
- `opencomputers-redstone-signaller`
