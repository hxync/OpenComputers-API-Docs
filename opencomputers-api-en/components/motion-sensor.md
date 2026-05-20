# motion_sensor

## Summary

The `motion_sensor` component detects movement from living entities near the sensor and emits `computer.signal` events when movement crosses a configured threshold. It is useful for alarms, security systems, trap logic, and automation that reacts to nearby players or mobs.

Detection is not global. The sensor only watches an eight-block radius around itself, only tracks living entities, ignores invisible ones, and requires line of sight to the target.

## Availability

- Repository: `opencomputers`
- Component name: `motion_sensor`
- Typical host: motion sensor block or compatible adapter connection

## Runtime Behavior

The sensor checks for motion every 10 ticks and emits a signal named `motion` when:

- a living entity enters detectable range for the first time, or
- a tracked entity moves farther than the current sensitivity threshold.

The signal payload contains relative coordinates from the sensor center:

```lua
"motion", dx, dy, dz
```

On installations where username reporting is enabled in config, the event also appends the entity name:

```lua
"motion", dx, dy, dz, name
```

## Usage

```lua
local component = require("component")
local motion = component.motion_sensor

if not motion then
  error("motion_sensor component not available")
end
```

Example event loop:

```lua
local event = require("event")

while true do
  local _, _, dx, dy, dz, name = event.pull("motion")
  print("Motion at:", dx, dy, dz, name or "")
end
```

## API

### getSensitivity

- Syntax: `motion.getSensitivity()`
- Returns: `number`
- Purpose: Gets the current movement threshold.

Smaller values make the sensor more sensitive. Larger values require a bigger position change before another signal is emitted for an already tracked entity.

Example:

```lua
print("Sensitivity:", motion.getSensitivity())
```

### setSensitivity

- Syntax: `motion.setSensitivity(value)`
- Returns: `number`
- Purpose: Changes the movement threshold and returns the previous value.

Parameter:

- `value:number`: desired sensitivity threshold.

Behavior:

- The source clamps the value to a minimum of `0.2`.
- There is no explicit upper limit in the component itself.

Example:

```lua
local old = motion.setSensitivity(0.25)
print("Old sensitivity:", old)
print("New sensitivity:", motion.getSensitivity())
```

## Related

- `component`
- `event`
- `redstone`
