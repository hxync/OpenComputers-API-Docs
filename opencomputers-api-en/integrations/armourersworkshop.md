# armourersworkshop

## Summary

This page documents the Armourer's Workshop integration exposed through OpenComputers and Computronics. It provides one component for reading and changing mannequin pose data from Lua.

## Availability

- Dependency: `armourersworkshop`
- Label: `integration-required`

## Component Name

- `mannequin`

## What This Integration Is Good For

- Driving animated display mannequins from timers or events.
- Resetting mannequins to known poses after scripted scenes.
- Synchronizing decorative poses with redstone or other automation logic.

## Component

### mannequin

`mannequin` is exposed on Armourer's Workshop mannequin blocks.

#### Valid Part Names

All pose callbacks use one of these lower-case part names:

- `head`
- `chest`
- `left_arm`
- `right_arm`
- `left_leg`
- `right_leg`

#### getRotation(part)

- Syntax: `getRotation(part: string): number, number, number`
- Purpose: Return the current X, Y, and Z rotation of one mannequin part.
- Parameters:
  - `part`: target part name.
- Returns:
  - Three rotation values in degrees.
- Usage notes:
  - The source stores rotations internally in radians, but the Lua API converts them to degrees before returning them.
  - Passing an invalid part name raises `invalid mannequin part`.

#### getRotationX(part)

- Syntax: `getRotationX(part: string): number`
- Purpose: Return the current X-axis rotation of one mannequin part.
- Parameters:
  - `part`: target part name.
- Returns:
  - One rotation value in degrees.
- Usage notes:
  - Passing an invalid part name raises `invalid mannequin part`.

#### getRotationY(part)

- Syntax: `getRotationY(part: string): number`
- Purpose: Return the current Y-axis rotation of one mannequin part.
- Parameters:
  - `part`: target part name.
- Returns:
  - One rotation value in degrees during normal use.
- Usage notes:
  - Passing an invalid part name raises `invalid mannequin part`.

#### getRotationZ(part)

- Syntax: `getRotationZ(part: string): number`
- Purpose: Return the current Z-axis rotation of one mannequin part.
- Parameters:
  - `part`: target part name.
- Returns:
  - One rotation value in degrees during normal use.
- Usage notes:
  - Passing an invalid part name raises `invalid mannequin part`.

#### setRotation(part[, x[, y[, z]]])

- Syntax: `setRotation(part: string[, x: number | nil[, y: number | nil[, z: number | nil]]])`
- Purpose: Change one or more axes of a mannequin part in a single call.
- Parameters:
  - `part`: target part name.
  - `x`: new X rotation in degrees, or `nil` to leave it unchanged.
  - `y`: new Y rotation in degrees, or `nil` to leave it unchanged.
  - `z`: new Z rotation in degrees, or `nil` to leave it unchanged.
- Returns:
  - No useful return values on success.
- Usage notes:
  - Each supplied axis must be between `-180` and `180`.
  - Values are converted from degrees to radians before being written into the mannequin.
  - Invalid angles raise `rotation must be between -180 and 180`.
  - `setRotation("head", nil, 45, nil)` is valid and changes only the Y axis.

#### setRotationX(part, x)

- Syntax: `setRotationX(part: string, x: number)`
- Purpose: Change only the X-axis rotation of one part.
- Parameters:
  - `part`: target part name.
  - `x`: new X rotation in degrees.
- Returns:
  - No useful return values on success.
- Usage notes:
  - The valid range is `-180` to `180`.
  - Invalid angles raise `rotation must be between -180 and 180`.

#### setRotationY(part, y)

- Syntax: `setRotationY(part: string, y: number)`
- Purpose: Change only the Y-axis rotation of one part.
- Parameters:
  - `part`: target part name.
  - `y`: new Y rotation in degrees.
- Returns:
  - No useful return values on success.
- Usage notes:
  - The valid range is `-180` to `180`.
  - Invalid angles raise `rotation must be between -180 and 180`.

#### setRotationZ(part, z)

- Syntax: `setRotationZ(part: string, z: number)`
- Purpose: Change only the Z-axis rotation of one part.
- Parameters:
  - `part`: target part name.
  - `z`: new Z rotation in degrees.
- Returns:
  - No useful return values on success.
- Usage notes:
  - The valid range is `-180` to `180`.
  - Invalid angles raise `rotation must be between -180 and 180`.

#### parts

- Syntax: `parts`
- Purpose: Return the built-in list of valid mannequin part names.
- Returns:
  - An indexed Lua table containing `head`, `chest`, `left_arm`, `right_arm`, `left_leg`, and `right_leg`.
- Usage notes:
  - Use this getter when you want to iterate over every supported part without hardcoding the list.

## Example

```lua
local component = require("component")
local mannequin = component.mannequin

for index, name in pairs(mannequin.parts) do
  print(index, name)
end

mannequin.setRotation("head", 0, 30, 0)
mannequin.setRotationX("left_arm", -45)
mannequin.setRotationZ("right_arm", 20)

local x, y, z = mannequin.getRotation("head")
print("Head rotation:", x, y, z)
```

## Related

- `opencomputers-redstone-signaller`
