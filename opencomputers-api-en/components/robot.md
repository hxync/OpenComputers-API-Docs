# robot

## Summary

This page documents the robot-specific callbacks exposed by the real `robot` component. These are the controls that exist only on robots, in addition to the shared agent, inventory, tank, redstone, and upgrade-specific APIs documented on other pages.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `robot`
- Backing implementation: `li.cil.oc.server.component.Robot`
- Typical host: robot itself

## What This Page Covers

- Robot movement
- Turning
- Tool durability queries
- Activity light color

## What It Does Not Repeat

Robots also inherit many other callback groups, including:

- shared agent actions such as `swing`, `use`, and `place`
- internal inventory control
- external inventory interaction from upgrades
- tank-related methods
- redstone-related methods

Those are documented on their own pages to avoid duplicating the same API multiple times.

## Direction Rules

Movement directions use the usual OpenComputers robot-relative side constants.

Typical meanings:

- `sides.front`
- `sides.back`
- `sides.up`
- `sides.down`

Horizontal left and right movement depends on the robot's current facing.

## API

### getLightColor

- Syntax: `robot.getLightColor()`
- Returns: `number`
- Purpose: Returns the current activity light color as `0xRRGGBB`.

### setLightColor

- Syntax: `robot.setLightColor(value)`
- Parameters:
  - `value:number`: RGB color encoded as `0xRRGGBB`.
- Returns: `number`
- Purpose: Sets the activity light color and returns the applied value.

Behavior details:

- The call pauses the computer briefly.

### durability

- Syntax: `robot.durability()`
- Returns:
  - Success: `number`
  - Failure: `nil, "no tool equipped"` or `nil, "tool cannot be damaged"`
- Purpose: Reads the durability state of the currently equipped tool.

Return meaning:

- The exact numeric meaning depends on the registered durability provider for that tool.
- It is intended as a normalized durability readout for scripts, not necessarily the raw vanilla damage value.

### move

- Syntax: `robot.move(direction)`
- Parameters:
  - `direction:number`: movement direction relative to the robot.
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Tries to move the robot by one block in the requested direction.

Possible failure reasons include:

- `already moving`
- `not enough energy`
- `impossible move`
- a collision reason returned by the movement/block-content checks

Behavior details:

- Movement consumes robot energy.
- If movement fails after energy was reserved, the energy is refunded.
- Failed collision-style moves trigger a short pause and particle effect.

### turn

- Syntax: `robot.turn(clockwise)`
- Parameters:
  - `clockwise:boolean`: `true` turns right, `false` turns left.
- Returns:
  - Success: `true`
  - Failure: `nil, "not enough energy"`
- Purpose: Rotates the robot in place.

Behavior details:

- Turning consumes robot energy.
- The computer pauses for the configured turn delay.

## Example

```lua
local robot = require("robot")
local sides = require("sides")

print(string.format("Light: 0x%06X", robot.getLightColor()))
robot.setLightColor(0x33CCFF)

local durability, err = robot.durability()
print("Durability:", durability, err)

local ok, why = robot.move(sides.front)
print("Move result:", ok, why)

if ok then
  robot.turn(true)
end
```

## Related

- `opencomputers-agent`
- `opencomputers-inventory-control`
- `opencomputers-redstone-vanilla`
