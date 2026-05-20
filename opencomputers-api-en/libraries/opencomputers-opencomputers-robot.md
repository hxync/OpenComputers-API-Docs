# robot

## Summary

High-level convenience wrappers around the robot component for movement, inventory, tools, and tank operations.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\lua\component\robot\lib\robot.lua`

## Usage

```lua
local robot = require("robot")
```

## API

### robot.back()

- Description: Move the robot one block backward.

```lua
robot.back()
```

### robot.compare(fuzzy)

- Description: Compare the selected stack with the inventory or block target in front of the robot.
- Parameters:
  - `fuzzy`

```lua
robot.compare(nil)
```

### robot.compareDown(fuzzy)

- Description: Compare the selected stack with the target below the robot.
- Parameters:
  - `fuzzy`

```lua
robot.compareDown(nil)
```

### robot.compareFluid()

- Description: Compare the selected tank contents with the front-side fluid target.

```lua
robot.compareFluid()
```

### robot.compareFluidDown()

- Description: Compare the selected tank contents with the fluid target below the robot.

```lua
robot.compareFluidDown()
```

### robot.compareFluidTo(...)

- Description: Compare the selected tank contents with another robot tank.
- Parameters:
  - `...`

```lua
robot.compareFluidTo(nil)
```

### robot.compareFluidUp()

- Description: Compare the selected tank contents with the fluid target above the robot.

```lua
robot.compareFluidUp()
```

### robot.compareTo(...)

- Description: Compare the selected inventory slot with another robot slot.
- Parameters:
  - `...`

```lua
robot.compareTo(nil)
```

### robot.compareUp(fuzzy)

- Description: Compare the selected stack with the target above the robot.
- Parameters:
  - `fuzzy`

```lua
robot.compareUp(nil)
```

### robot.count(...)

- Description: Return how many items are stored in a slot.
- Parameters:
  - `...`

```lua
robot.count(nil)
```

### robot.detect()

- Description: Check whether the front side is obstructed by a block or entity.

```lua
robot.detect()
```

### robot.detectDown()

- Description: Check whether the space below the robot is occupied.

```lua
robot.detectDown()
```

### robot.detectUp()

- Description: Check whether the space above the robot is occupied.

```lua
robot.detectUp()
```

### robot.down()

- Description: Move the robot one block downward.

```lua
robot.down()
```

### robot.drain(count)

- Description: Drain fluid from the front side into the selected tank.
- Parameters:
  - `count`

```lua
robot.drain(nil)
```

### robot.drainDown(count)

- Description: Drain fluid from below into the selected tank.
- Parameters:
  - `count`

```lua
robot.drainDown(nil)
```

### robot.drainUp(count)

- Description: Drain fluid from above into the selected tank.
- Parameters:
  - `count`

```lua
robot.drainUp(nil)
```

### robot.drop(count)

- Description: Drop items from the selected slot to the front side.
- Parameters:
  - `count`

```lua
robot.drop(nil)
```

### robot.dropDown(count)

- Description: Drop items from the selected slot downward.
- Parameters:
  - `count`

```lua
robot.dropDown(nil)
```

### robot.dropUp(count)

- Description: Drop items from the selected slot upward.
- Parameters:
  - `count`

```lua
robot.dropUp(nil)
```

### robot.durability()

- Description: Return durability information for the installed robot tool upgrade.

```lua
robot.durability()
```

### robot.fill(count)

- Description: Push fluid from the selected tank into the front-side target.
- Parameters:
  - `count`

```lua
robot.fill(nil)
```

### robot.fillDown(count)

- Description: Push fluid from the selected tank into the target below the robot.
- Parameters:
  - `count`

```lua
robot.fillDown(nil)
```

### robot.fillUp(count)

- Description: Push fluid from the selected tank into the target above the robot.
- Parameters:
  - `count`

```lua
robot.fillUp(nil)
```

### robot.forward()

- Description: Move the robot one block forward.

```lua
robot.forward()
```

### robot.getLightColor()

- Description: Return the RGB light color currently configured on the robot.

```lua
robot.getLightColor()
```

### robot.inventorySize()

- Description: Return how many inventory slots the robot currently has.

```lua
robot.inventorySize()
```

### robot.level()

- Description: Return the robot experience level when an experience upgrade is present, otherwise `0`.

```lua
robot.level()
```

### robot.name()

- Description: Return the robot's configured name.

```lua
robot.name()
```

### robot.place(side, sneaky)

- Description: Place or use the selected item against the front side, optionally with sub-side targeting and sneaking.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.place(nil, nil)
```

### robot.placeDown(side, sneaky)

- Description: Place or use the selected item against the target below the robot.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.placeDown(nil, nil)
```

### robot.placeUp(side, sneaky)

- Description: Place or use the selected item against the target above the robot.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.placeUp(nil, nil)
```

### robot.select(...)

- Description: Get or set the selected inventory slot through the robot component.
- Parameters:
  - `...`

```lua
robot.select(nil)
```

### robot.selectTank(tank)

- Description: Get or set the selected tank.
- Parameters:
  - `tank`

```lua
robot.selectTank(nil)
```

### robot.setLightColor(value)

- Description: Set the robot light color and return the component call result.
- Parameters:
  - `value`

```lua
robot.setLightColor(nil)
```

### robot.space(...)

- Description: Return how many more items can still fit into a slot.
- Parameters:
  - `...`

```lua
robot.space(nil)
```

### robot.suck(count)

- Description: Pull loose items or inventory contents from the front side into the selected slot.
- Parameters:
  - `count`

```lua
robot.suck(nil)
```

### robot.suckDown(count)

- Description: Pull loose items or inventory contents from below into the selected slot.
- Parameters:
  - `count`

```lua
robot.suckDown(nil)
```

### robot.suckUp(count)

- Description: Pull loose items or inventory contents from above into the selected slot.
- Parameters:
  - `count`

```lua
robot.suckUp(nil)
```

### robot.swing(side, sneaky)

- Description: Use the installed tool against the block or entity in front of the robot.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.swing(nil, nil)
```

### robot.swingDown(side, sneaky)

- Description: Use the installed tool against the target below the robot.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.swingDown(nil, nil)
```

### robot.swingUp(side, sneaky)

- Description: Use the installed tool against the target above the robot.
- Parameters:
  - `side`
  - `sneaky`

```lua
robot.swingUp(nil, nil)
```

### robot.tankCount()

- Description: Return how many fluid tanks the robot currently exposes.

```lua
robot.tankCount()
```

### robot.tankLevel(...)

- Description: Return how much fluid is stored in a tank.
- Parameters:
  - `...`

```lua
robot.tankLevel(nil)
```

### robot.tankSpace(...)

- Description: Return how much additional fluid a tank can still accept.
- Parameters:
  - `...`

```lua
robot.tankSpace(nil)
```

### robot.transferFluidTo(...)

- Description: Move fluid from the selected tank into another robot tank.
- Parameters:
  - `...`

```lua
robot.transferFluidTo(nil)
```

### robot.transferTo(...)

- Description: Move items from the selected slot into another robot slot.
- Parameters:
  - `...`

```lua
robot.transferTo(nil)
```

### robot.turnAround()

- Description: Turn the robot 180 degrees by performing two same-direction quarter turns.

```lua
robot.turnAround()
```

### robot.turnLeft()

- Description: Rotate the robot 90 degrees to the left.

```lua
robot.turnLeft()
```

### robot.turnRight()

- Description: Rotate the robot 90 degrees to the right.

```lua
robot.turnRight()
```

### robot.up()

- Description: Move the robot one block upward.

```lua
robot.up()
```

### robot.use(side, sneaky, duration)

- Description: Activate or use the selected item toward the front side, optionally sneaking or holding the action.
- Parameters:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.use(nil, nil, nil)
```

### robot.useDown(side, sneaky, duration)

- Description: Activate or use the selected item downward.
- Parameters:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.useDown(nil, nil, nil)
```

### robot.useUp(side, sneaky, duration)

- Description: Activate or use the selected item upward.
- Parameters:
  - `side`
  - `sneaky`
  - `duration`

```lua
robot.useUp(nil, nil, nil)
```

## Notes

- These wrappers translate directional convenience calls such as `forward`, `swingUp`, or `dropDown` into the underlying component methods with fixed side constants.
