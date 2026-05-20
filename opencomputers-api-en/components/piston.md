# piston

## Summary

The `piston` component is the Lua interface exposed by the piston upgrade. It lets supported hosts push one adjacent block using vanilla piston rules.

## Availability

- Repository: `opencomputers`
- Lua component name: `piston`
- Typical hosts: robots, drones, tablets, and microcontrollers with the piston upgrade installed

## What It Does

- Tries to extend in one direction as if a vanilla piston were mounted on the host.
- Consumes component energy before the push succeeds.
- Removes the directly targeted block after a successful piston-style extension, matching the upgrade's implementation flow.

## Direction Rules

- For rotatable hosts such as robots and microcontrollers, omitting the side uses the host's front direction.
- For drones, omitting the side defaults to `south`.
- For tablets, the push origin follows the player's position, with a small special case for downward pushes from taller player viewpoints.

## API

### push

- Syntax: `piston.push([side])`
- Parameters:
  - `side:number`: optional target side.
- Returns: `boolean`
- Purpose: Tries to push the block on the specified side of the host.

Return value meanings:

- `true`: a push occurred successfully.
- `false`: the target space was air, there was not enough energy, or vanilla piston logic refused the move.

## Practical Notes

- The upgrade follows vanilla piston extension constraints, so immovable blocks and blocked push chains still fail.
- A successful push pauses the calling context briefly.
- Each successful attempt consumes the configured piston energy cost.

## Example

```lua
local component = require("component")
local sides = require("sides")
local piston = component.piston

local ok = piston.push(sides.front)
print("Push succeeded:", ok)
```

## Related

- `robot`
- `microcontroller`
- `tablet`
