# opencomputers-agent

## Summary

This filename does not represent a real standalone Lua component.

It documents the shared world-interaction callbacks provided by agent-style hosts, primarily:

- `robot`
- `drone`

These callbacks are exposed through the host's actual component, such as `component.robot` or `component.drone`.

## Hosts Covered

### robot

- Real Lua component name: `robot`
- Side arguments are interpreted relative to the robot's orientation.
- World interaction includes robot animation and normal robot action delays.

### drone

- Real Lua component name: `drone`
- Side arguments use world-relative directions.
- World interaction pauses are effectively doubled compared with the base action delay.

## What These Callbacks Do

- Read the agent's configured display name.
- Swing the equipped tool or attack toward a side.
- Use the equipped item or activate a block toward a side.
- Place the selected inventory item into the world.

## Shared Direction Rules

The first argument of `swing`, `use`, and `place` is the primary action direction.

- On robots, use the normal OpenComputers relative side constants.
- On drones, side values are interpreted directly.

The optional `face` argument is the side of the targeted block face that should be clicked.

Behavior notes:

- If `face` is omitted, the implementation tries the primary side first, then other nearby faces when applicable.
- `sneaky=true` makes the fake player perform the action while sneaking.

## Return Conventions

`swing` and `use` return a success flag plus a reason string when applicable.

Typical reason strings include:

- `entity`
- `block`
- `fire`
- `air`
- `item_interacted`
- `block_activated`
- `item_placed`
- `item_used`

`place` returns only a boolean on success, or `nil, reason` on certain failures.

## API

### name

- Syntax: `agent.name()`
- Returns: `string`
- Purpose: Returns the agent's configured name.

On robots this is the robot label shown by the block and GUI. On drones it is the drone's configured name.

### swing

- Syntax: `agent.swing(side[, face[, sneaky]])`
- Parameters:
  - `side:number`: primary direction to attack or left-click.
  - `face:number`: optional target face on the destination block.
  - `sneaky:boolean`: optional sneak state, default `false`.
- Returns: `boolean, string`
- Purpose: Performs a left-click style action toward the chosen side.

Behavior details:

- If an entity is hit first, the agent attacks it with the currently equipped tool.
- If a block is hit, the agent tries to break or damage it.
- If nothing is hit, the implementation still checks for close living entities and fire blocks.
- Minecarts are hit repeatedly in quick succession to improve break reliability.

Common results:

- `true, "entity"`: attacked an entity.
- `true, "block"`: hit or broke a block.
- `true, "fire"`: extinguished fire.
- `false, "air"`: nothing useful was hit.

### use

- Syntax: `agent.use(side[, face[, sneaky[, duration]]])`
- Parameters:
  - `side:number`: primary direction to right-click toward.
  - `face:number`: optional clicked face.
  - `sneaky:boolean`: optional sneak state, default `false`.
  - `duration:number`: optional item-use duration, default `0`.
- Returns:
  - Success: `boolean, string`
  - Failure: `false`
- Purpose: Performs a right-click style action toward the chosen side.

Behavior details:

- Can interact with entities.
- Can activate blocks.
- Can place blocks in air when the host is allowed to do so.
- Falls back to using the equipped item even when no block or entity is hit.

Common success reasons:

- `item_interacted`
- `block_activated`
- `item_placed`
- `item_used`

`duration` matters for items with sustained use behavior, such as bows or charge-up tools.

### place

- Syntax: `agent.place(side[, face[, sneaky]])`
- Parameters:
  - `side:number`: primary placement direction.
  - `face:number`: optional clicked face on the target block.
  - `sneaky:boolean`: optional sneak state, default `false`.
- Returns:
  - Success: `true`
  - Failure: `false`
  - Empty-slot failure: `nil, "nothing selected"`
- Purpose: Places the currently selected inventory item into the world.

Behavior details:

- The selected inventory slot must contain a placeable item stack.
- If the target block is reachable, the fake player uses a normal block-placement interaction.
- If nothing is hit, some hosts can still place into air when their rules allow it.

## Example

```lua
local component = require("component")
local sides = require("sides")

local agent = component.robot or component.drone
if not agent then
  error("no agent-style component available")
end

print("Agent name:", agent.name())

local ok, why = agent.swing(sides.front)
print("Swing result:", ok, why)

local used, how = agent.use(sides.front, sides.front, false, 0)
print("Use result:", used, how)
```

## Related

- `robot`
- `drone`
- `opencomputers-inventory-control`
