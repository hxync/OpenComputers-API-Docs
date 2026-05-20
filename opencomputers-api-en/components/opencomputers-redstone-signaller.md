# opencomputers-redstone-signaller

## Summary

This page documents the wake-up control callbacks shared by `redstone` components. They do not read or write redstone on their own; instead, they configure when incoming redstone changes should wake sleeping computers on the component network.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `redstone`
- Backing trait: `RedstoneSignaller`

## What It Does

- Stores a numeric wake threshold.
- Emits `redstone_changed` computer signals when monitored redstone state changes.
- Starts sleeping computers when a redstone input rises from below the threshold to at least the threshold.

By default, wake-up packets are sent only to neighboring computers on the same component network.

## API

### getWakeThreshold

- Syntax: `redstone.getWakeThreshold()`
- Returns: `number`
- Purpose: Reads the current wake threshold used for redstone-triggered startup.

### setWakeThreshold

- Syntax: `redstone.setWakeThreshold(threshold)`
- Parameters:
  - `threshold:number`: minimum new input value that may trigger wake-up when crossed upward.
- Returns: `number`
- Purpose: Sets a new wake threshold and returns the previous threshold.

## Wake Logic

Wake-up only happens when both of these are true:

- the old input value was lower than the threshold;
- the new input value is greater than or equal to the threshold.

This means a steady high signal does not repeatedly wake computers unless it drops below threshold and rises again.

## Related Signals

Redstone-aware components emit:

- Wired analog change: `"redstone_changed", side, oldValue, newValue`
- Bundled change: `"redstone_changed", side, oldValue, newValue, color`
- Wireless change: `"redstone_changed", "wireless", oldValue, newValue`

## Example

```lua
local component = require("component")
local redstone = component.redstone

local old = redstone.setWakeThreshold(10)
print("Old threshold:", old)
print("Current threshold:", redstone.getWakeThreshold())
```

## Related

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-bundled`
- `opencomputers-redstone-wireless`
