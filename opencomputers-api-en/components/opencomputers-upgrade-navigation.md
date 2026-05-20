# opencomputers-upgrade-navigation

## Summary

This page documents the `navigation` component exposed by the navigation upgrade.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `navigation`
- Typical hosts: robots, drones, tablets, and other rotatable hosts that support the navigation upgrade

## What It Does

- Reports the host's relative map position.
- Reports the host's current facing direction.
- Reports the upgrade's operating range.
- Scans for nearby waypoint blocks.

## Position Model

The navigation upgrade stores a map center internally. `getPosition()` reports coordinates relative to that center.

If the host is outside the upgrade's mapped area, `getPosition()` fails with `nil, "out of range"`.

## API

### getPosition

- Syntax: `navigation.getPosition()`
- Returns:
  - Success: `relativeX, y, relativeZ`
  - Failure: `nil, "out of range"`
- Purpose: Reports the host's relative position inside the navigation map.

### getFacing

- Syntax: `navigation.getFacing()`
- Returns: `number`
- Purpose: Returns the host's current facing as a ForgeDirection ordinal.

### getRange

- Syntax: `navigation.getRange()`
- Returns: `number`
- Purpose: Returns the navigation upgrade's operational radius.

### findWaypoints

- Syntax: `navigation.findWaypoints(range)`
- Returns:
  - Success: `table`
  - Failure: `nil, "not enough energy"`
- Purpose: Finds waypoint blocks within the requested radius.

Behavior:

- Requested range is clamped to the tier-two wireless maximum.
- A non-positive range returns an empty array immediately.
- Successful scans pause the calling context briefly and consume energy based on the scan range.

Each result entry is a table containing:

- `position`: `{dx, dy, dz}` relative to the host
- `redstone`: waypoint redstone input strength
- `label`: waypoint label text
- `address`: waypoint component address

The reported position points to the block in front of the waypoint, not the waypoint block itself.

## Example

```lua
local component = require("component")
local navigation = component.navigation

local x, y, z = navigation.getPosition()
print("Relative position:", x, y, z)
print("Facing:", navigation.getFacing())
print("Range:", navigation.getRange())

local waypoints, reason = navigation.findWaypoints(32)
if not waypoints then
  error(reason)
end

for i, waypoint in ipairs(waypoints) do
  print(i, waypoint.label, table.unpack(waypoint.position))
end
```

## Related

- `waypoint`
- `robot`
- `tablet`
