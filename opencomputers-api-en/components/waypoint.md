# waypoint

## Summary

The `waypoint` component exposes the editable label stored on a waypoint block. Waypoints themselves are mainly consumed by navigation upgrades, which can discover them and report a position marker to robots, drones, and other mobile devices.

This component API is intentionally small. It exists so programs can read or rename waypoints without opening the block GUI manually.

## Availability

- Repository: `opencomputers`
- Component name: `waypoint`
- Typical host: waypoint block

## Important Notes

- Navigation upgrades detect the block **in front of** the waypoint, not the block the waypoint occupies.
- A waypoint label is limited to 32 characters in source.
- Changing the label pauses the computer briefly.

## Usage

```lua
local component = require("component")
local waypoint = component.waypoint

if not waypoint then
  error("waypoint component not available")
end
```

## API

### getLabel

- Syntax: `waypoint.getLabel()`
- Returns: `string`
- Purpose: Reads the waypoint's current label.

The label is often used by navigation programs to decide how to treat a destination, for example as a pickup point, drop-off point, charger, or staging area.

Example:

```lua
print("Waypoint label:", waypoint.getLabel())
```

### setLabel

- Syntax: `waypoint.setLabel(value)`
- Returns: no direct return values
- Purpose: Changes the waypoint label.

Parameter:

- `value:string`: new label text.

Behavior:

- The label is truncated to 32 characters.
- The implementation pauses the computer for `0.5` seconds.

Example:

```lua
waypoint.setLabel("north_dock_input")
print("New label:", waypoint.getLabel())
```

## Related

- `navigation`
- `robot`
- `drone`
- `component`
