# camera

## Summary

The `camera` component on a Computronics robot is a short-range distance sensor. It casts a ray and returns how far away the first solid block is along that line.

This is useful for obstacle detection, alignment, simple wall-following, and checking ceiling or floor clearance.

## Availability

- Repository: `computronics`
- Component name: `camera`
- Typical host: robot with the Computronics camera upgrade installed

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
- Optional `x` and `y` offsets change the ray direction.
- Each offset must stay in the range `-1.0` to `1.0`.
- If the ray does not hit a block within range, the methods return `-1`.

The robot version can scan:

- forward using its current facing direction
- straight upward
- straight downward

## API

### distance

- Syntax: `camera.distance([x, y])`
- Returns: `number`
- Purpose: Returns the distance to the first block hit in front of the robot.

Parameters:

- `x:number` optional: horizontal view offset.
- `y:number` optional: vertical view offset.

Behavior:

- With no arguments, the ray is cast straight ahead.
- If either offset is outside `-1.0` to `1.0`, the scan does not produce a hit and the method returns `-1`.
- If no block is hit within range, the method returns `-1`.

Example:

```lua
local d = camera.distance()
if d >= 0 then
  print("Wall ahead at", d, "blocks")
else
  print("No block detected ahead")
end
```

Example: look slightly to the right.

```lua
print("Right offset distance:", camera.distance(0.4, 0.0))
```

### distanceUp

- Syntax: `camera.distanceUp([x, y])`
- Returns: `number`
- Purpose: Returns the distance to the first block hit above the robot.

Parameters:

- `x:number` optional: horizontal offset across the upward view plane.
- `y:number` optional: second view-plane offset used while looking upward.

This is useful for ceiling detection, elevator docking, and vertical clearance checks.

Example:

```lua
local ceiling = camera.distanceUp()
if ceiling >= 0 then
  print("Ceiling distance:", ceiling)
end
```

### distanceDown

- Syntax: `camera.distanceDown([x, y])`
- Returns: `number`
- Purpose: Returns the distance to the first block hit below the robot.

Parameters:

- `x:number` optional: horizontal offset across the downward view plane.
- `y:number` optional: second view-plane offset used while looking downward.

This is useful for cliff detection, floor finding, and checking drop depth.

Example:

```lua
local floor = camera.distanceDown()
if floor < 0 then
  print("No floor found within range")
else
  print("Floor distance:", floor)
end
```

## Notes

- These methods report block hits, not entities.
- The ray starts slightly outside the robot body so the host block itself does not interfere with the reading.

## Related

- `component`
- `radar`
