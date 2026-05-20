# opencomputers-upgrade-generator

## Summary

This page documents the `generator` component exposed by the upgrade generator installed in robots and similar mobile hosts.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `generator`
- Typical hosts: robots, drones, and other agent-style hosts that support the generator upgrade

## What It Does

- Stores one queued stack of burnable fuel.
- Burns queued fuel items over time to recharge the component buffer.
- Lets Lua insert fuel from the selected slot and remove queued fuel back to inventory.

## Queue Rules

- Only one fuel type may be queued at a time.
- Queue size is limited by the fuel item's normal stack size.
- Fuels with container items may require inventory space for those containers.

## API

### insert

- Syntax: `generator.insert([count])`
- Returns:
  - Success: `true, insertedCount`
  - Failure: `nil, reason` or `false, reason`
- Purpose: Moves fuel from the selected inventory slot into the generator queue.

Possible failure reasons:

- `selected slot is empty`
- `selected slot does not contain fuel`
- `different fuel type already queued`
- `queue is full`
- `no space in inventory for fuel containers`

`count` defaults to `64`.

### count

- Syntax: `generator.count()`
- Returns:
  - Queue empty: `0`
  - Queue non-empty: `queuedCount, fuelDisplayName`
- Purpose: Reports how much fuel is currently queued.

### remove

- Syntax: `generator.remove([count])`
- Returns:
  - Success: `true` or `true, removedCount`
  - Failure: `false, reason`
- Purpose: Removes queued fuel items and returns them to the player's inventory.

Possible failure reasons:

- `queue is empty`
- `removing this fuel requires the appropriate container in the selected slot`
- `no inventory space available for fuel`

Behavior details:

- Passing `0` or a negative count is allowed and returns `true`.
- If the fuel type uses an empty container, the selected slot must hold matching empty containers before removal is allowed.

## Practical Notes

- The queued stack is dropped into the world if the component disconnects while fuel remains queued.
- The generator consumes queued items automatically when its current burn time runs out.
- Energy gain per tick is controlled by the OpenComputers generator efficiency setting.

## Example

```lua
local component = require("component")
local robot = require("robot")
local generator = component.generator

robot.select(1)

local ok, value = generator.insert(4)
print("Insert result:", ok, value)

local queued, name = generator.count()
print("Queued fuel:", queued, name)

local removed, countOrReason = generator.remove(2)
print("Remove result:", removed, countOrReason)
```

## Related

- `robot`
- `drone`
- `computer`
