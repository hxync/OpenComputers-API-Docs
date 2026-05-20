# opencomputers-net-splitter

## Summary

This page documents the `net_splitter` component. A net splitter is a network block that can open or close each of its six sides independently, letting you physically segment OpenComputers cable networks.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `net_splitter`
- Backing implementation: `li.cil.oc.common.tileentity.NetSplitter`
- Typical host: placed net splitter block

## What It Does

- Opens or closes network connectivity on individual sides.
- Reports the current effective open or closed state of all sides.
- Can switch all six sides at once from a single Lua call.

## Side Numbers

Use the normal OpenComputers side constants `0` through `5`.

- `0`: bottom
- `1`: top
- `2`: north
- `3`: south
- `4`: west
- `5`: east

Using `require("sides")` is recommended instead of hard-coding numbers.

## Redstone Inversion

When the block receives a redstone signal, it enters inverted mode.

- Inverted mode flips the meaning of each stored side state.
- `getSides()`, `open()`, `close()`, and `setSides()` all work with the effective state you currently see.
- That means `open(side)` still means "make this side logically open" even while the block is inverted.

## API

### getSides

- Syntax: `netSplitter.getSides()`
- Returns: `table`
- Purpose: Returns the current effective state of every side.

Return format:

- The returned table is keyed by side number.
- Each value is `true` for open or `false` for closed.

### setSides

- Syntax: `netSplitter.setSides(settings)`
- Parameters:
  - `settings:table`: table keyed by side number, with boolean values.
- Returns: `table`
- Purpose: Sets all side states in one call and returns the previous effective state table.

Behavior details:

- Missing side entries are treated as `false`.
- Because of that, omitted sides are closed.
- Values that are not booleans are also treated as `false`.

### open

- Syntax: `netSplitter.open(side)`
- Parameters:
  - `side:number`: side to open.
- Returns:
  - Success: `boolean`
  - Failure: `nil, "invalid direction"`
- Purpose: Opens one side and reports whether the effective state actually changed.

Return meaning:

- `true`: the side changed from closed to open.
- `false`: the side was already effectively open.

### close

- Syntax: `netSplitter.close(side)`
- Parameters:
  - `side:number`: side to close.
- Returns:
  - Success: `boolean`
  - Failure: `nil, "invalid direction"`
- Purpose: Closes one side and reports whether the effective state actually changed.

Return meaning:

- `true`: the side changed from open to closed.
- `false`: the side was already effectively closed.

## Example

```lua
local component = require("component")
local sides = require("sides")

local netSplitter = component.net_splitter

print("Initial side states:")
for side, isOpen in pairs(netSplitter.getSides()) do
  print(side, isOpen)
end

local changed = netSplitter.close(sides.north)
print("North changed:", changed)

local previous = netSplitter.setSides({
  [sides.top] = true,
  [sides.bottom] = true,
  [sides.north] = false,
  [sides.south] = false,
  [sides.west] = true,
  [sides.east] = true
})

print("Previous top state:", previous[sides.top])
```

## Related

- `modem`
- `tunnel`
