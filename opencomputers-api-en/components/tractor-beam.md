# tractor_beam

## Summary

The `tractor_beam` component is the pickup interface exposed by the tractor beam upgrade. It allows supported hosts to collect nearby dropped item entities without moving onto them manually.

## Availability

- Repository: `opencomputers`
- Lua component name: `tractor_beam`
- Typical hosts: robots, drones, and player-carried hosts with the tractor beam upgrade installed

## What It Does

- Searches for dropped item entities within a radius of `3` blocks around the host.
- Randomly chooses one eligible item stack from the nearby pool.
- Attempts to collect that stack into the host inventory or player inventory.

## API

### suck

- Syntax: `tractor_beam.suck()`
- Returns: `boolean`
- Purpose: Tries to collect one nearby dropped item stack.

Return value meanings:

- `true`: at least part of one nearby stack was collected.
- `false`: no eligible dropped item could be collected.

## Practical Notes

- Only items whose pickup delay has expired are eligible.
- The upgrade works on dropped entities in an area, not on blocks or inventories.
- A successful pickup pauses the calling context using the normal OpenComputers suck delay.
- On drone-style hosts, insertion prefers the currently selected slot order first, then wraps around the rest of the inventory.

## Example

```lua
local component = require("component")
local beam = component.tractor_beam

if beam.suck() then
  print("Picked up an item.")
else
  print("Nothing collectable in range.")
end
```

## Related

- `robot`
- `drone`
- `opencomputers-inventory-world-control`
