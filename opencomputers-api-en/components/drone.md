# drone

## Summary

The `drone` component exposes drone-specific status and flight controls. It lets a program read the drone's current movement state, adjust its acceleration target, move its flight target, and update the visible status text and flap light color.

These callbacks are only available on a drone itself, so scripts usually access them from the drone's onboard computer rather than through a stationary machine.

## Availability

- Repository: `opencomputers`
- Component name: `drone`
- Typical host: drone

## Usage

```lua
local component = require("component")
local drone = component.drone

if not drone then
  error("drone component not available")
end
```

## API

### getAcceleration

- Syntax: `drone.getAcceleration()`
- Returns: `number`
- Purpose: Gets the drone's currently configured acceleration target in meters per second squared.

This is the value previously accepted by `setAcceleration`, after internal clamping and conversion.

Example:

```lua
print("Acceleration:", drone.getAcceleration(), "m/s^2")
```

### getLightColor

- Syntax: `drone.getLightColor()`
- Returns: `number`
- Purpose: Reads the current flap light color as an integer RGB value in `0xRRGGBB` format.

Example:

```lua
print(string.format("Light color: 0x%06X", drone.getLightColor()))
```

### getMaxVelocity

- Syntax: `drone.getMaxVelocity()`
- Returns: `number`
- Purpose: Gets the drone's maximum flight speed in meters per second.

This is the speed cap imposed by the drone entity, not the current movement speed.

Example:

```lua
print("Max speed:", drone.getMaxVelocity(), "m/s")
```

### getOffset

- Syntax: `drone.getOffset()`
- Returns: `number`
- Purpose: Gets the remaining distance from the drone's current position to its current target position.

This is useful for movement loops that wait until the drone finishes traveling to a destination.

Example:

```lua
while drone.getOffset() > 0.2 do
  os.sleep(0.1)
end
print("Target reached")
```

### getStatusText

- Syntax: `drone.getStatusText()`
- Returns: `string`
- Purpose: Reads the short text currently shown in the drone GUI.

Example:

```lua
print("Status:", drone.getStatusText())
```

### getVelocity

- Syntax: `drone.getVelocity()`
- Returns: `number`
- Purpose: Gets the drone's current flight speed in meters per second.

The value is calculated from the entity's current motion vector.

Example:

```lua
print("Current speed:", drone.getVelocity(), "m/s")
```

### move

- Syntax: `drone.move(dx, dy, dz)`
- Returns: no direct return values
- Purpose: Offsets the drone's current target position by the specified amounts.

Parameters:

- `dx:number`: relative movement along the X axis.
- `dy:number`: relative movement along the Y axis.
- `dz:number`: relative movement along the Z axis.

Behavior:

- This does not teleport the drone.
- It changes the target coordinates the drone will try to reach.
- The actual flight path depends on the drone's physics, acceleration, and current motion.

Example:

```lua
drone.move(0, 3, 0)
while drone.getOffset() > 0.2 do
  os.sleep(0.1)
end
print("Moved up by three blocks")
```

### setAcceleration

- Syntax: `drone.setAcceleration(value)`
- Returns: `number`
- Purpose: Sets the drone's target acceleration in meters per second squared and returns the resulting value.

Parameters:

- `value:number`: desired acceleration.

The API accepts and returns per-second units, while the entity stores an internal per-tick value. OpenComputers handles the conversion for you.

Example:

```lua
local applied = drone.setAcceleration(4)
print("Applied acceleration:", applied, "m/s^2")
```

### setLightColor

- Syntax: `drone.setLightColor(value)`
- Returns: `number`
- Purpose: Changes the flap light color and returns the applied RGB value.

Parameters:

- `value:number`: color encoded as `0xRRGGBB`.

Notes:

- This call pauses the current computer for a short time (`0.1` seconds in source) to model the world interaction cost.

Example:

```lua
drone.setLightColor(0x00FF00)
print("Light updated")
```

### setStatusText

- Syntax: `drone.setStatusText(value)`
- Returns: `string`
- Purpose: Changes the GUI status text shown for the drone and returns the applied text.

Parameters:

- `value:string`: text to display.

Notes:

- This call also incurs a short world interaction pause (`0.1` seconds in source).

Example:

```lua
drone.setStatusText("Delivering")
print("New status:", drone.getStatusText())
```

## Related

- `computer`
- `navigation`
- `modem`
- `robot`
