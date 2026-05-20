# printer3d

## Summary

The `printer3d` component configures and starts OpenComputers 3D print jobs. It lets you build a print model out of cuboid shapes, define labels and tooltips, configure light and redstone behavior, choose collision rules, and commit the model for repeated printing.

## Availability

- Repository: `opencomputers`
- Component name: `printer3d`
- Typical host: placed OpenComputers 3D printer

## Usage

```lua
local component = require("component")
local printer = component.printer3d

if not printer then
  error("printer3d component not installed")
end
```

## Printer Model Basics

- A model is split into two shape sets:
  - `stateOff`
  - `stateOn`
- At least one shape must exist in `stateOff` for the model to be printable.
- `stateOn` is optional and is used for active-state variants.
- Each shape is one cuboid.
- Shape coordinates are clamped to the `0` through `16` voxel range before being converted to normalized block coordinates.

## Complexity And Limits

- Maximum shapes per state are limited by `Settings.get.maxPrintComplexity`.
- `getMaxShapeCount()` returns that configured value.
- Labels are truncated to `24` characters.
- Tooltips are truncated to `128` characters.
- Light level is clamped to `0` through `Settings.get.maxPrintLightLevel`.
- Redstone level is clamped to `0` through `15`.
- `commit(count)` clamps `count` to `0` through `Integer.MAX_VALUE`.

## Material And Ink

The 3D printer consumes:

- chamelium-derived print material
- printable ink
- energy from the OC network buffer

Model cost is based on:

- total shape volume for material
- total shape surface area for ink
- an extra material surcharge when custom redstone strength is used
- a noclip multiplier when either state is non-collidable

## API

### reset

- Syntax: `printer.reset()`
- Returns: no meaningful value
- Purpose: Clears the current model configuration and cancels further queued printing.

Behavior:

- The current physical print already in progress is allowed to finish.
- The shape sets, label, tooltip, light, redstone, and collision settings are reset because a new empty `PrintData` object is created.

### setLabel

- Syntax: `printer.setLabel(value)`
- Returns: no meaningful value
- Purpose: Sets the printed block label.

Parameters:

- `value:string`: desired label, truncated to `24` characters.

Behavior:

- Empty strings become `nil`.

### getLabel

- Syntax: `printer.getLabel()`
- Returns: `string|nil`
- Purpose: Returns the current pending print label.

### setTooltip

- Syntax: `printer.setTooltip(value)`
- Returns: no meaningful value
- Purpose: Sets the printed block tooltip.

Parameters:

- `value:string`: tooltip text, truncated to `128` characters.

Behavior:

- Empty strings become `nil`.

### getTooltip

- Syntax: `printer.getTooltip()`
- Returns: `string|nil`
- Purpose: Returns the current pending tooltip.

### setLightLevel

- Syntax: `printer.setLightLevel(value)`
- Returns: no meaningful value
- Purpose: Sets emitted light level for the printed block.

Parameters:

- `value:number`: requested light level. The implementation clamps it to the configured maximum.

### getLightLevel

- Syntax: `printer.getLightLevel()`
- Returns: `number`
- Purpose: Returns the current configured light level.

### setRedstoneEmitter

- Syntax: `printer.setRedstoneEmitter(value)`
- Returns: no meaningful value
- Purpose: Configures active-state redstone output.

Parameters:

- `value:boolean|number`:
  - `true` sets redstone level to `15`
  - `false` sets redstone level to `0`
  - a number is clamped to `0` through `15`

### isRedstoneEmitter

- Syntax: `printer.isRedstoneEmitter()`
- Returns: `boolean, number`
- Purpose: Returns whether the print emits redstone and which strength it uses.

### setButtonMode

- Syntax: `printer.setButtonMode(value)`
- Returns: no meaningful value
- Purpose: Sets whether the printed block should automatically return from the active state to the inactive state.

### isButtonMode

- Syntax: `printer.isButtonMode()`
- Returns: `boolean`
- Purpose: Returns the current button-mode flag.

### setCollidable

- Syntax: `printer.setCollidable(collideOff, collideOn)`
- Returns: no meaningful value
- Purpose: Sets whether the printed block collides in the off and on states.

Parameters:

- `collideOff:boolean`
- `collideOn:boolean`

Behavior:

- Internally this flips `noclipOff` and `noclipOn`.

### isCollidable

- Syntax: `printer.isCollidable()`
- Returns: `boolean, boolean`
- Purpose: Returns the off-state and on-state collision flags.

### addShape

- Syntax: `printer.addShape(minX, minY, minZ, maxX, maxY, maxZ, texture[, state=false][, tint])`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Adds one cuboid shape to the current model.

Parameters:

- `minX`, `minY`, `minZ:number`: first corner in voxel coordinates
- `maxX`, `maxY`, `maxZ:number`: second corner in voxel coordinates
- `texture:string`: texture name, truncated to `64` characters
- `state:boolean` optional: `false` for the off-state shape set, `true` for the on-state shape set
- `tint:number` optional: tint value applied to the shape

Behavior:

- Coordinates are clamped to `0` through `16`.
- `minZ` and `maxZ` are flipped internally using `16 - value`.
- If any dimension collapses to zero thickness, the callback throws `empty block`.
- If the shape count is already above the configured maximum, the callback returns `nil, "model too complex"`.

Example:

```lua
assert(printer.addShape(0, 0, 0, 16, 2, 16, "opencomputers:block/print"))
```

### getShapeCount

- Syntax: `printer.getShapeCount()`
- Returns: `offCount, onCount`
- Purpose: Returns the number of shapes currently stored in the off and on states.

### getMaxShapeCount

- Syntax: `printer.getMaxShapeCount()`
- Returns: `number`
- Purpose: Returns the configured maximum shape count per state.

### commit

- Syntax: `printer.commit([count])`
- Returns:
  - Success: `true`
  - Failure: `nil, "model invalid"`
- Purpose: Marks the current model active and begins printing up to the requested number of copies over time.

Parameters:

- `count:number` optional: number of copies to queue. Defaults to `1`.

Behavior:

- The model must have at least one `stateOff` shape.
- Both shape sets must stay within the configured complexity limit.
- `count <= 0` effectively disables further printing by leaving `isActive` false.

### status

- Syntax: `printer.status()`
- Returns:
  - While printing: `"busy", progress`
  - Idle with valid model: `"idle", true`
  - Idle with invalid model: `"idle", false`
- Purpose: Reports printer state and either progress or model validity.

Behavior:

- `progress` is a percentage from `0` to `100`.

## Example

```lua
printer.reset()
printer.setLabel("Status Block")
printer.setTooltip("Printed by OC")
printer.setLightLevel(8)
printer.setRedstoneEmitter(15)
printer.setButtonMode(true)
printer.setCollidable(true, true)

assert(printer.addShape(0, 0, 0, 16, 4, 16, "opencomputers:block/print"))
assert(printer.addShape(4, 4, 4, 12, 12, 12, "opencomputers:block/print", true, 0xFF0000))

print("Shapes:", printer.getShapeCount())
assert(printer.commit(1))
```

## Related

- `component`
- `opencomputers-openos-print3d`
