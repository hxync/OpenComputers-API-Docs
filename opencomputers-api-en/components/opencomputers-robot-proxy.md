# opencomputers-robot-proxy

## Summary

This page documents the externally visible `robot` component callbacks provided by a placed robot block. They let nearby machines inspect the robot's name and running state, start or stop it, and rename it while it is powered down.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `robot`
- Backing implementation: `li.cil.oc.common.tileentity.RobotProxy`
- Typical host: placed robot block seen from the outside

## What Makes This Different From `robot`

The normal [`robot`](./robot.md) page documents callbacks available to programs running on the robot itself.

This page covers the proxy view seen by neighboring computers through component scanning:

- start the robot
- stop the robot
- read whether it is running
- read or change its name

It does not expose the robot's onboard movement, inventory, or tool-use callbacks.

## API

### start

- Syntax: `robotProxy.start()`
- Returns: `boolean`
- Purpose: Attempts to start the robot and reports whether the running state changed.

Behavior notes:

- If the robot successfully starts, the return value is `true`.
- If the robot was already running, or is paused in a way that prevents starting, the return value is `false`.

### stop

- Syntax: `robotProxy.stop()`
- Returns: `boolean`
- Purpose: Attempts to stop the robot and reports whether the running state changed.

If the robot was already stopped, the call returns `false`.

### isRunning

- Syntax: `robotProxy.isRunning()`
- Returns: `boolean`
- Purpose: Returns whether the robot's machine is currently running.

### getName

- Syntax: `robotProxy.getName()`
- Returns: `string`
- Purpose: Returns the robot's current name.

### setName

- Syntax: `robotProxy.setName(name)`
- Parameters:
  - `name:string`: new robot name.
- Returns:
  - Success: `string`
  - Failure: `nil, "is running"`
- Purpose: Renames the robot and returns the previous name.

Restriction:

- The robot must not be running when you rename it.

## Example

```lua
local component = require("component")

for address in component.list("robot") do
  local proxy = component.proxy(address)
  print(address, proxy.getName(), proxy.isRunning())
end
```

## Related

- `robot`
- `computer`
