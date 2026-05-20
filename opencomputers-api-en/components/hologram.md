# hologram

## Summary

The `hologram` component controls a hologram projector's voxel buffer, palette, scale, translation, and rotation. It is one of OpenComputers' few true 3D display APIs: instead of drawing text cells, you manipulate individual voxels in a `48 x 32 x 48` volume.

Tier 1 projectors support on/off voxels only. Tier 2 projectors support multiple color values, user-editable palette entries, and programmable rotation.

## Availability

- Repository: `opencomputers`
- Component name: `hologram`
- Typical host: hologram projector

## Usage

```lua
local component = require("component")
local hologram = component.hologram

if not hologram then
  error("hologram component not available")
end
```

## Coordinate Model

- Lua coordinates are `1`-based.
- Valid ranges are:
  - `x = 1..48`
  - `y = 1..32`
  - `z = 1..48`
- Voxel value `0` means empty.
- On tier 1, usable voxel values are effectively `0` and `1`.
- On tier 2, usable values are `0` through `3`:
  - `0` = empty,
  - `1..3` = palette colors.

Invalid coordinates raise argument/index errors. Invalid voxel values raise `"invalid value"`.

## API

### clear

- Syntax: `hologram.clear()`
- Returns: no direct return values
- Purpose: Clears the entire hologram buffer.

This turns every voxel off at once.

Example:

```lua
hologram.clear()
```

### get

- Syntax: `hologram.get(x, y, z)`
- Returns: `number`
- Purpose: Reads the current voxel value at one position.

Parameters:

- `x:number`, `y:number`, `z:number`: voxel coordinates.

Example:

```lua
print("Voxel value:", hologram.get(24, 16, 24))
```

### set

- Syntax: `hologram.set(x, y, z, value)`
- Returns: no direct return values
- Purpose: Sets one voxel.

Parameters:

- `x:number`, `y:number`, `z:number`: voxel coordinates.
- `value:number|boolean`: voxel state or color index.

Notes:

- Passing `true` is shorthand for value `1`.
- Passing `false` is shorthand for value `0`.
- Numeric values must stay within the projector's supported range.

Example:

```lua
hologram.set(24, 16, 24, 1)
```

### fill

- Syntax: `hologram.fill(x, z[, minY], maxY, value)`
- Returns: no direct return values
- Purpose: Fills a vertical interval inside one `x,z` column.

Parameters:

- `x:number`, `z:number`: column position.
- `minY:number` optional: lower bound. Defaults to `1`.
- `maxY:number`: upper bound.
- `value:number|boolean`: voxel value to write.

Behavior:

- `minY` and `maxY` are clamped into the valid `1..32` range.
- If `minY > maxY` after processing, the call raises `"interval is empty"`.

Example:

```lua
hologram.fill(24, 24, 1, 16, 1)
```

Example with implicit `minY = 1`:

```lua
hologram.fill(24, 24, 32, true)
```

### setRaw

- Syntax: `hologram.setRaw(data)`
- Returns: no direct return values
- Purpose: Replaces projector data from a raw byte array.

Parameter:

- `data:string`: byte array where each byte represents one voxel color value.

Important details:

- Source layout is nested in `x, z, y` order.
- This is the fastest way to upload large voxel scenes.
- The implementation pauses the computer briefly after the upload.

Example:

```lua
local bytes = {}
for i = 1, 48 * 48 * 32 do
  bytes[i] = string.char(0)
end
hologram.setRaw(table.concat(bytes))
```

### copy

- Syntax: `hologram.copy(x, z, sx, sz, tx, tz)`
- Returns: no direct return values
- Purpose: Copies a rectangular area of columns by a horizontal translation.

Parameters:

- `x:number`, `z:number`: source start column.
- `sx:number`, `sz:number`: size of the column area along X and Z.
- `tx:number`, `tz:number`: translation offsets.

Behavior:

- Copying is column-based, so the entire height of each affected column is moved.
- Non-positive sizes do nothing.
- A zero translation does nothing.
- Larger copies pause the computer proportionally to the affected area.

Example:

```lua
hologram.copy(10, 10, 8, 8, 12, 0)
```

### getScale

- Syntax: `hologram.getScale()`
- Returns: `number`
- Purpose: Reads the current render scale.

### setScale

- Syntax: `hologram.setScale(value)`
- Returns: no direct return values
- Purpose: Changes the projector's render scale.

Parameter:

- `value:number`: desired scale.

Behavior:

- The value is clamped to a minimum of `1/3`.
- The maximum is limited by the projector tier and mod settings.
- Larger scale increases energy usage.

Example:

```lua
hologram.setScale(1.5)
print("Scale:", hologram.getScale())
```

### getTranslation

- Syntax: `hologram.getTranslation()`
- Returns: `number, number, number`
- Purpose: Reads the current render offset.

### setTranslation

- Syntax: `hologram.setTranslation(tx, ty, tz)`
- Returns: no direct return values
- Purpose: Changes the rendered hologram offset relative to the projector.

Parameters:

- `tx:number`: horizontal X offset.
- `ty:number`: vertical offset.
- `tz:number`: horizontal Z offset.

Behavior:

- X and Z are clamped symmetrically to the configured maximum translation.
- Y is clamped into `0 .. maxTranslation * 2`.

Example:

```lua
hologram.setTranslation(0.2, 0.5, -0.2)
```

### maxDepth

- Syntax: `hologram.maxDepth()`
- Returns: `number`
- Purpose: Gets the hologram's supported color depth level.

Practical meaning:

- tier 1 returns `1`,
- tier 2 returns `2`.

### getPaletteColor

- Syntax: `hologram.getPaletteColor(index)`
- Returns: `number`
- Purpose: Reads one palette color as a normal `0xRRGGBB` RGB value.

Parameter:

- `index:number`: palette entry index, starting at `1`.

Invalid indices raise an out-of-range error.

### setPaletteColor

- Syntax: `hologram.setPaletteColor(index, value)`
- Returns: `number`
- Purpose: Changes one palette entry and returns the previously stored value.

Parameters:

- `index:number`: palette entry index, starting at `1`.
- `value:number`: RGB color in `0xRRGGBB` format.

For scripts that need to verify the visible color after the change, calling `getPaletteColor(index)` is the safest follow-up.

Example:

```lua
hologram.setPaletteColor(1, 0x00FF00)
```

### setRotation

- Syntax: `hologram.setRotation(angle, x, y, z)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Sets the base rotation axis and angle for the rendered hologram.

Parameters:

- `angle:number`: rotation angle in degrees. Source wraps it modulo `360`.
- `x:number`, `y:number`, `z:number`: rotation axis vector.

Tier note:

- Tier 1 projectors return `nil, "not supported"`.
- Tier 2 projectors support this feature.

### setRotationSpeed

- Syntax: `hologram.setRotationSpeed(speed, x, y, z)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Sets continuous rotation speed and axis.

Parameters:

- `speed:number`: degrees per cycle parameter, clamped to `-1440 .. 1440` in source.
- `x:number`, `y:number`, `z:number`: rotation axis vector.

Tier note:

- Tier 1 projectors return `nil, "not supported"`.
- Tier 2 projectors support this feature.

### getDimensions

- Syntax: `hologram.getDimensions()`
- Returns: `number, number, number`
- Purpose: Returns the projector dimensions as `x, y, z`.

This always reports `48, 32, 48`.

Example:

```lua
local w, h, d = hologram.getDimensions()
print("Hologram size:", w, h, d)
```

## Related

- `component`
- `robot`
- `drone`
