# radar

## Summary

The robot `radar` component scans nearby entities and item drops around the host. It is useful for player detection, mob awareness, dropped-item pickup logic, and proximity-based automation.

The scan result is returned as a list-like Lua table of entry tables.

## Availability

- Repository: `computronics`
- Component name: `radar`
- Typical host: robot with the Computronics radar upgrade installed

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
- Each OpenComputers scan pauses the calling context for `0.5` seconds when it actually runs.
- Scan energy cost increases with requested distance and target type.

For reliable behavior, pass a distance from `1` to `8`. The implementation accepts other integers, but spatial scanning still caps to the configured maximum range.

## Result Format

All four methods return a list-like table of entry tables.

Entity and player entry fields:

- `name:string`
- `distance:number`
- `x:number`, `y:number`, `z:number` only when the configuration does not force distance-only radar results

Item entry fields:

- `name:string`: registry name of the item stack
- `label:string`: display name
- `size:number`: stack size
- `damage:number`: item damage / metadata
- `hasTag:boolean`
- `distance:number`
- `x:number`, `y:number`, `z:number` only when distance-only mode is disabled

## API

### getEntities

- Syntax: `radar.getEntities([distance])`
- Returns: `table`
- Purpose: Returns players and living mobs detected in range.

Energy behavior:

- Cost scales as `distance * 1.75 * radarEnergyCost`.

If the component does not have enough energy, this method returns an empty table instead of an error.

Example:

```lua
for _, entry in ipairs(radar.getEntities()) do
  print(entry.name, entry.distance)
end
```

### getPlayers

- Syntax: `radar.getPlayers([distance])`
- Returns: `table`
- Purpose: Returns detected players only.

Energy behavior:

- Cost scales as `distance * 1.0 * radarEnergyCost`.

If the component does not have enough energy, this method returns an empty table.

Example:

```lua
local players = radar.getPlayers(4)
for _, player in ipairs(players) do
  print("Player:", player.name, player.distance)
end
```

### getMobs

- Syntax: `radar.getMobs([distance])`
- Returns: `table`
- Purpose: Returns detected living mobs only.

Energy behavior:

- Cost scales as `distance * 1.0 * radarEnergyCost`.

If the component does not have enough energy, this method returns an empty table.

Example:

```lua
local mobs = radar.getMobs()
print("Mob count:", #mobs)
```

### getItems

- Syntax: `radar.getItems([distance])`
- Returns: `table`
- Purpose: Returns detected dropped item entities.

Energy behavior:

- Cost scales as `distance * 2.0 * radarEnergyCost`.

If the component does not have enough energy, this method returns an empty table.

Example:

```lua
for _, item in ipairs(radar.getItems(3)) do
  print(item.label, item.size, item.distance)
end
```

## Example

```lua
local hostilesNearby = false

for _, mob in ipairs(radar.getMobs(5)) do
  if mob.distance < 3 then
    hostilesNearby = true
    print("Nearby mob:", mob.name)
  end
end

if hostilesNearby then
  print("Close-range alert")
end
```

## Notes

- The robot version returns a list-like Lua array of results.
- Entity coordinates are only included when the server configuration allows more than distance-only radar data.

## Related

- `component`
- `camera`
