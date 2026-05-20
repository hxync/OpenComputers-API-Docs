# opencomputers-screen

## Summary

This page documents the screen-block specific callbacks layered on top of the base `screen` component. These callbacks control how physical interaction opens the GUI versus sending touch input.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `screen`
- Backing implementation: `li.cil.oc.common.component.Screen`
- Typical host: placed screen block

## What It Does

- Reports whether touch interaction mode is inverted.
- Lets Lua invert how normal click and sneak-click interactions are interpreted.

## Touch Mode Meaning

Normally, screens with a keyboard attached tend to open the GUI on normal activation and treat sneaking activation as direct touch input. Inverted touch mode swaps that behavior.

The precise player-facing outcome still depends on whether a keyboard is attached and on the usual screen interaction rules, but this flag is the programmable toggle that flips the default mapping.

## API

### isTouchModeInverted

- Syntax: `screen.isTouchModeInverted()`
- Returns: `boolean`
- Purpose: Returns whether touch mode is currently inverted.

### setTouchModeInverted

- Syntax: `screen.setTouchModeInverted(value)`
- Parameters:
  - `value:boolean`: desired inversion state.
- Returns: `boolean`
- Purpose: Sets touch mode inversion and returns the previous state.

## Example

```lua
local component = require("component")
local screen = component.screen

print("Was inverted:", screen.setTouchModeInverted(true))
print("Is inverted now:", screen.isTouchModeInverted())
```

## Related

- `screen`
- `keyboard`
