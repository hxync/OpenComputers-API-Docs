# radar

## Summary

The block `radar` component scans nearby players, mobs, and item drops around a placed radar block. It is useful for area monitoring, alarm systems, item tracking, and stationary perimeter sensors.

Like the robot version, the result is returned as a list-like table of entry tables.

## Availability

- Repository: `computronics`
- Component name: `radar`
- Typical host: placed Computronics radar block

## Usage

```lua
local component = require("component")
local radar = component.radar

if not radar then
  error("radar component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Scan Model

- The default maximum scan radius is `8` blocks.
- Passing no argument uses that configured maximum radius.
- A successful OpenComputers scan pauses the context for `0.5` seconds.
- The block version can draw power from either the OpenComputers connector buffer or its own internal battery.

For predictable behavior, pass distances from `1` to `8`.

## Result Format

All methods return a list-like table indexed from `1`.

Entity and player entry fields:

- `name:string`
- `distance:number`
- `x:number`, `y:number`, `z:number` when distance-only mode is disabled

Item entry fields:

- `name:string`
- `label:string`
- `size:number`
- `damage:number`
- `hasTag:boolean`
- `distance:number`
- `x:number`, `y:number`, `z:number` when distance-only mode is disabled

## API

### getEntities

- Syntax: `radar.getEntities([distance])`
- Returns: `table`
- Purpose: Returns detected players and living mobs.

Energy behavior:

- Cost scales as `distance * 1.75 * radarEnergyCost`.

If neither the OC buffer nor the internal battery can pay the cost, the method returns an empty table.

Example:

```lua
for _, entity in pairs(radar.getEntities()) do
  print(entity.name, entity.distance)
end
```

### getPlayers

- Syntax: `radar.getPlayers([distance])`
- Returns: `table`
- Purpose: Returns detected players only.

Energy behavior:

- Cost scales as `distance * 1.0 * radarEnergyCost`.

If the scan cannot be powered, the method returns an empty table.

Example:

```lua
local players = radar.getPlayers()
for _, player in pairs(players) do
  print(player.name)
end
```

### getMobs

- Syntax: `radar.getMobs([distance])`
- Returns: `table`
- Purpose: Returns detected living mobs only.

Energy behavior:

- Cost scales as `distance * 1.0 * radarEnergyCost`.

If the scan cannot be powered, the method returns an empty table.

Example:

```lua
local mobs = radar.getMobs(6)
print("Mob entries:", #mobs)
```

### getItems

- Syntax: `radar.getItems([distance])`
- Returns: `table`
- Purpose: Returns detected dropped item entities.

Energy behavior:

- Cost scales as `distance * 2.0 * radarEnergyCost`.

If the scan cannot be powered, the method returns an empty table.

Example:

```lua
for _, item in pairs(radar.getItems(4)) do
  print(item.label, item.size)
end
```

## Notes

- The block radar returns results as an integer-keyed Lua table built from a Java set. Treat it as a list, but do not rely on stable ordering between scans.
- Coordinate fields appear only when the configuration allows more than distance-only output.

## Related

- `component`
- `camera`
