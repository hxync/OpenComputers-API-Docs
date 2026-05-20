# opencomputers-redstone-bundled

## Summary

This page documents the bundled-signal extensions exposed by advanced `redstone` components. Bundled redstone carries sixteen independent color channels per side and is available only when supported redstone integration mods are present.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `redstone`
- Backing trait: `RedstoneBundled`

## What It Does

- Reads bundled input on one color, one side, or all sides.
- Reads bundled output on one color or one side.
- Sets bundled output per color, per side, or with a nested side table.
- Emits `redstone_changed` signals that include a color index for bundled changes.

Color indexes range from `0` to `15`.

## Signal Form

Bundled input changes emit:

```lua
"redstone_changed", side, oldValue, newValue, color
```

The extra `color` argument identifies which bundled channel changed.

## API

### getBundledInput

- Syntax:
  - `redstone.getBundledInput()`
  - `redstone.getBundledInput(side)`
  - `redstone.getBundledInput(side, color)`
- Returns:
  - No arguments: `table` of `side -> colorTable`
  - One argument: `table` of `color -> value`
  - Two arguments: `number`
- Purpose: Reads bundled input values.

Input values are the maximum merged value from supported bundled redstone providers on that side.

### getBundledOutput

- Syntax:
  - `redstone.getBundledOutput()`
  - `redstone.getBundledOutput(side)`
  - `redstone.getBundledOutput(side, color)`
- Returns:
  - No arguments: table-like snapshot for all sides
  - One argument: `table` of `color -> value`
  - Two arguments: `number`
- Purpose: Reads bundled output values.

Version note:

- In this source version, the zero-argument path appears to use an inconsistent internal snapshot. For precise results, prefer `getBundledOutput(side)` or `getBundledOutput(side, color)`.

### setBundledOutput

- Syntax:
  - `redstone.setBundledOutput(side, color, value)`
  - `redstone.setBundledOutput(side, colorTable)`
  - `redstone.setBundledOutput(sideTable)`
- Returns:
  - Single-color form: previous value on that channel
  - Side-table form: previous values for that side
  - Full-table form: previous values snapshot
- Purpose: Updates bundled redstone outputs.

Accepted table shapes:

- `colorTable`: `color -> value`
- `sideTable`: `side -> colorTable`

## Practical Notes

- Bundled channels are independent; setting one color does not change the others.
- Like normal redstone output, writes may pause briefly if redstone delay is enabled.
- The advanced redstone controller reports a width of `16` channels and capacity `65536` in device info.

## Example

```lua
local component = require("component")
local sides = require("sides")
local redstone = component.redstone

local old = redstone.setBundledOutput(sides.left, 3, 255)
print("Old color 3 value on the left:", old)

local left = redstone.getBundledOutput(sides.left)
print("Left side color 3 now:", left[3])

local input = redstone.getBundledInput(sides.right, 5)
print("Right side bundled input color 5:", input)
```

## Related

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-wireless`
