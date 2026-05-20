# opencomputers-upgrade-sign-in-adapter

## Summary

This page documents the `sign` component exposed by an adapter when used with nearby signs.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `sign`
- Typical host: adapter block

## What It Does

- Reads text from a sign on a specified side of the adapter.
- Writes text to a sign on a specified side of the adapter.
- Can also target the host block itself if the host tile entity is a sign.

## Text Rules

- Sign text is read and returned as one string with newline separators.
- Written text is split into at most four lines.
- Each line is truncated to `15` characters.
- Missing lines are filled with empty strings.

## API

### getValue

- Syntax: `sign.getValue(side)`
- Returns:
  - Success: `string`
  - Failure: `nil, "no sign"`
- Purpose: Reads the text from the sign on the specified side.

### setValue

- Syntax: `sign.setValue(side, value)`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Writes text to the sign on the specified side and returns the applied text.

Possible failure reasons:

- `no sign`
- `not allowed`

`not allowed` means the fake-player permission checks or sign-change events denied the update.

## Example

```lua
local component = require("component")
local sides = require("sides")
local sign = component.sign

local text, reason = sign.getValue(sides.north)
print("Current text:", text, reason)

local updated, err = sign.setValue(sides.north, "Line 1\nLine 2")
print("Updated text:", updated, err)
```

## Related

- `opencomputers-upgrade-sign-in-rotatable`
- `adapter`
