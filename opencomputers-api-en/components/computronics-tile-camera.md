# camera

## Summary

The block-based `camera` component is a fixed rangefinder from Computronics. It measures the distance to the first solid block hit by a ray projected from the camera block's facing direction.

Compared with the robot version, this block variant only scans forward, but it is useful as a mounted sensor in doors, hallways, docking stations, and automation lines.

## Availability

- Repository: `computronics`
- Component name: `camera`
- Typical host: placed Computronics camera block

## Usage

```lua
local component = require("component")
local camera = component.camera

if not camera then
  error("camera component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Raycasting Model

- The default maximum range is `32` blocks.
- Optional `x` and `y` offsets adjust the ray within the camera view.
- Each offset must stay in the range `-1.0` to `1.0`.
- If no block is hit within range, the method returns `-1`.

The block always scans along its placed facing direction.

## API

### distance

- Syntax: `camera.distance([x, y])`
- Returns: `number`
- Purpose: Returns the distance to the first block hit by the camera.

Parameters:

- `x:number` optional: horizontal view offset.
- `y:number` optional: vertical view offset.

Behavior:

- With no arguments, the camera scans straight forward.
- If either offset lies outside `-1.0` to `1.0`, the method returns `-1`.
- If no block is hit within range, the method returns `-1`.

Example:

```lua
local d = camera.distance()
if d >= 0 then
  print("Detected block at", d, "blocks")
else
  print("Nothing detected in range")
end
```

Example: sample slightly above center.

```lua
print("Upper view distance:", camera.distance(0.0, -0.35))
```

## Notes

- This component measures blocks, not entities.
- In environments where the camera also drives redstone output refresh, the redstone side behavior is separate from the Lua callback and does not change the return format of `distance()`.

## Related

- `component`
- `radar`
