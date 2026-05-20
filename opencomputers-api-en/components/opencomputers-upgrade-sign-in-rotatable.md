# opencomputers-upgrade-sign-in-rotatable

## Summary

This page documents the `sign` component exposed by mobile or rotatable hosts that use the sign upgrade.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `sign`
- Typical hosts: robots and other rotatable hosts with the sign upgrade installed

## What It Does

- Reads the text from the sign directly in front of the host.
- Writes new text to the sign directly in front of the host.

It uses the same text formatting and permission rules as the adapter-based sign interface.

## Text Rules

- The sign text is represented as a single newline-joined string.
- Written text is split into at most four lines.
- Each line is truncated to `15` characters.

## API

### getValue

- Syntax: `sign.getValue()`
- Returns:
  - Success: `string`
  - Failure: `nil, "no sign"`
- Purpose: Reads the text from the sign in front of the host.

### setValue

- Syntax: `sign.setValue(value)`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Writes text to the sign in front of the host and returns the applied text.

Possible failure reasons:

- `no sign`
- `not allowed`

## Example

```lua
local component = require("component")
local sign = component.sign

print("Current sign text:", sign.getValue())
print("Updated sign text:", sign.setValue("Dock A\nInput"))
```

## Related

- `opencomputers-upgrade-sign-in-adapter`
- `robot`
