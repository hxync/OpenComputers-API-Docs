# buildcraft

## Summary

This page documents the BuildCraft integrations exposed to Lua by OpenComputers and Computronics. The available components cover transport pipes, heat-capable machines, drone docking stations, and controllable tiles.

## Availability

- Dependency: `buildcraft`
- Label: `integration-required`

## Component Names

- `bc_pipe`
- `heatable_tile`
- `docking`
- `bc_controllable`

## What This Integration Is Good For

- Inspecting pipe connectivity, gates, and wire state.
- Monitoring the heat window of BuildCraft machines.
- Docking drones to item pipes and dropping items into them.
- Reading and changing machine control modes.

## Components

### bc_pipe

`bc_pipe` is exposed on BuildCraft pipe tiles.

#### `getPipeType()`

- Syntax: `getPipeType(): string | nil, string?`
- Purpose: Return the current pipe type name.
- Returns:
  - The pipe type enum name on success.
  - `nil, "none"` if the underlying lookup fails.
- Usage notes:
  - This is useful for distinguishing item, fluid, power, and structure pipe families.

#### `isPipeConnected(side)`

- Syntax: `isPipeConnected(side: number): boolean`
- Purpose: Check whether the pipe is connected on the specified side.
- Parameters:
  - `side: number`
    A ForgeDirection side index.
- Returns:
  - `true` if that side is connected.
  - `false` if it is not connected or if the underlying lookup fails.
- Usage notes:
  - Invalid side values do not surface a Lua-visible exception here. They simply fall back to `false`.

#### `isWired(color)`

- Syntax: `isWired(color: string): boolean`
- Purpose: Check whether the pipe has a wire of the specified color installed.
- Parameters:
  - `color: string`
    The exact BuildCraft `PipeWire` enum name, typically an uppercase color name.
- Returns:
  - `true` if the wire is installed.
  - `false` if it is missing or the color name is invalid.

#### `isWireActive(color)`

- Syntax: `isWireActive(color: string): boolean`
- Purpose: Check whether the specified pipe wire is currently active.
- Parameters:
  - `color: string`
    The exact BuildCraft `PipeWire` enum name, typically an uppercase color name.
- Returns:
  - `true` if the wire is active.
  - `false` if it is inactive or the color name is invalid.
- Usage notes:
  - `isWired()` only checks presence. `isWireActive()` checks live state.

#### `hasGate(side)`

- Syntax: `hasGate(side: number): boolean`
- Purpose: Check whether the specified side has a gate installed.
- Parameters:
  - `side: number`
    A ForgeDirection side index.
- Returns:
  - `true` if a gate is present on that side.
  - `false` if it is missing or the lookup fails.

### heatable_tile

`heatable_tile` is exposed on BuildCraft tiles implementing `IHeatable`.

#### `getMinHeatValue()`

- Syntax: `getMinHeatValue(): number`
- Purpose: Return the minimum meaningful heat value.
- Returns:
  - The lower bound where the machine starts entering its useful heat range.

#### `getIdealHeatValue()`

- Syntax: `getIdealHeatValue(): number`
- Purpose: Return the ideal working heat value.
- Returns:
  - The target heat level the machine prefers.

#### `getMaxHeatValue()`

- Syntax: `getMaxHeatValue(): number`
- Purpose: Return the maximum safe heat value.
- Returns:
  - The upper limit before heat should generally be treated as unsafe.

#### `getCurrentHeatValue()`

- Syntax: `getCurrentHeatValue(): number`
- Purpose: Return the machine's current heat.
- Returns:
  - The live heat value.
- Usage notes:
  - This is mainly intended for engines and other BuildCraft machines that operate within a heat window.
  - In practice it is usually more useful to compare this value against `getIdealHeatValue()` than only against the maximum.

### docking

`docking` is exposed by the Computronics drone docking upgrade for BuildCraft pipe docking stations.

#### `dock()`

- Syntax: `dock(): boolean[, string]`
- Purpose: Start docking the drone with a valid station, checking the block below first.
- Returns:
  - `true` if docking has started successfully.
  - `false, reason` on failure.
- Usage notes:
  - Returns `false, "drone is still moving"` if the drone has not come to rest yet.
  - Returns `false, "no non-occupied station found"` if no free station is available.
  - A successful call starts the docking process; it does not mean the drone is already fully docked in the same tick.

#### `dropItem(slot[, maxAmount[, color]])`

- Syntax: `dropItem(slot: number[, maxAmount: number[, color: number]]): number[, string]`
- Purpose: Inject items from the drone inventory into the attached item pipe while docked.
- Parameters:
  - `slot: number`
    Drone inventory slot, starting at `1`.
  - `maxAmount: number`
    Maximum number of items to inject. The driver clamps this to `0..64`, defaulting to `64`.
  - `color: number`
    Optional BuildCraft item color id. It only has an effect on tier 2 or higher drones.
- Returns:
  - The number of items actually injected on success.
  - `0, reason` on failure.
- Usage notes:
  - This only works when the drone is already docked to an item pipe.
  - Failure messages include `drone is still docking`, `drone is not docked`, `cannot inject items into pipe`, `invalid pipe type`, and `invalid/empty slot`.
  - If a color is supplied on a lower-tier drone, the color argument is ignored.

#### `release()`

- Syntax: `release(): boolean[, string]`
- Purpose: Undock the drone from the station.
- Returns:
  - `true` on success.
  - `0, reason` on failure.
- Usage notes:
  - Returns `0, "drone is still docking"` if docking has not finished yet.
  - Returns `0, "drone is not docked"` if the drone is not currently docked.

### bc_controllable

`bc_controllable` is exposed on BuildCraft tiles implementing `IControllable`.

#### `getControlMode()`

- Syntax: `getControlMode(): string`
- Purpose: Return the current control mode name.
- Returns:
  - The current BuildCraft `IControllable.Mode` enum name.

#### `acceptsControlMode(mode)`

- Syntax: `acceptsControlMode(mode: string): boolean`
- Purpose: Check whether the tile accepts the specified control mode.
- Parameters:
  - `mode: string`
    Exact BuildCraft `IControllable.Mode` enum name.
- Returns:
  - `true` if the tile supports that mode.
  - `false` if it does not.
- Usage notes:
  - Invalid enum names are not silently ignored here. They raise the underlying argument error.

#### `setControlMode(mode)`

- Syntax: `setControlMode(mode: string)`
- Purpose: Set the tile control mode.
- Parameters:
  - `mode: string`
    Exact BuildCraft `IControllable.Mode` enum name.
- Returns:
  - This callback does not return an additional success flag.
- Usage notes:
  - `mode` must be accepted by `IControllable.Mode.valueOf(...)`.
  - The safest workflow is to read the current value with `getControlMode()`, validate candidates with `acceptsControlMode()`, and then apply the new mode.

## Example

```lua
local component = require("component")

if component.isAvailable("bc_pipe") then
  local pipe = component.bc_pipe
  print("Pipe type:", pipe.getPipeType())
  print("North connected:", pipe.isPipeConnected(2))
end

if component.isAvailable("heatable_tile") then
  local heatable = component.heatable_tile
  print("Heat:", heatable.getCurrentHeatValue(), "/", heatable.getMaxHeatValue())
end
```

## Related

- `railcraft`
- `appeng`
