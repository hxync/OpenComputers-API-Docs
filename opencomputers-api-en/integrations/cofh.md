# cofh

## Summary

This page documents the CoFH integration exposed to Lua by OpenComputers. The available components cover Redstone Flux storage, CoFH machine energy info, ender transport channels, redstone control state, and secure access checks.

## Availability

- Dependency: `cofh`
- Label: `integration-required`

## Component Names

- `ender_energy`
- `ender_fluid`
- `ender_item`
- `energy_handler`
- `energy_info`
- `redstone_control`
- `secure_tile`

## What This Integration Is Good For

- Reading Redstone Flux storage from CoFH-compatible machines.
- Checking machine-side energy capacity from a specific direction.
- Inspecting CoFH machine GUI-style energy statistics.
- Reading and changing ender channel frequencies.
- Controlling CoFH redstone behavior modes from Lua.
- Checking ownership and access rules on secure tiles.

## Components

### ender_energy

`ender_energy` is exposed on CoFH ender energy transport blocks implementing `IEnderEnergyHandler`.

#### `canReceiveEnergy()`

- Syntax: `canReceiveEnergy(): boolean`
- Purpose: Return whether this tile can receive energy from its ender channel.
- Returns:
  - `true` if receiving is allowed.
  - `false` otherwise.

#### `canSendEnergy()`

- Syntax: `canSendEnergy(): boolean`
- Purpose: Return whether this tile can send energy into its ender channel.
- Returns:
  - `true` if sending is allowed.
  - `false` otherwise.

#### `getFrequency()`

- Syntax: `getFrequency(): number`
- Purpose: Return the current ender channel frequency.
- Returns:
  - Current frequency value.

#### `setFrequency(frequency)`

- Syntax: `setFrequency(frequency: number): boolean`
- Purpose: Change the ender channel frequency.
- Parameters:
  - `frequency: number`
    New channel frequency.
- Returns:
  - `true` if the frequency change succeeded.
  - `false` if the target tile rejected the new frequency.

#### `getChannelString()`

- Syntax: `getChannelString(): string`
- Purpose: Return the human-readable channel name.
- Returns:
  - Channel display string.

### ender_fluid

`ender_fluid` is exposed on CoFH ender fluid transport blocks implementing `IEnderFluidHandler`.

#### `canReceiveFluid()`

- Syntax: `canReceiveFluid(): boolean`
- Purpose: Return whether this tile can receive fluid through its ender channel.
- Returns:
  - `true` if fluid input is allowed.
  - `false` otherwise.

#### `canSendFluid()`

- Syntax: `canSendFluid(): boolean`
- Purpose: Return whether this tile can send fluid through its ender channel.
- Returns:
  - `true` if fluid output is allowed.
  - `false` otherwise.

#### `getFrequency()`

- Syntax: `getFrequency(): number`
- Purpose: Return the current ender channel frequency.
- Returns:
  - Current frequency value.

#### `setFrequency(frequency)`

- Syntax: `setFrequency(frequency: number): boolean`
- Purpose: Change the ender fluid channel frequency.
- Parameters:
  - `frequency: number`
    New channel frequency.
- Returns:
  - `true` if the frequency change succeeded.
  - `false` otherwise.

#### `getChannelString()`

- Syntax: `getChannelString(): string`
- Purpose: Return the channel name shown by the CoFH block.
- Returns:
  - Channel display string.

### ender_item

`ender_item` is exposed on CoFH ender item transport blocks implementing `IEnderItemHandler`.

#### `canReceiveItems()`

- Syntax: `canReceiveItems(): boolean`
- Purpose: Return whether this tile can receive items through its ender channel.
- Returns:
  - `true` if item input is allowed.
  - `false` otherwise.

#### `canSendItems()`

- Syntax: `canSendItems(): boolean`
- Purpose: Return whether this tile can send items through its ender channel.
- Returns:
  - `true` if item output is allowed.
  - `false` otherwise.

#### `getFrequency()`

- Syntax: `getFrequency(): number`
- Purpose: Return the current ender channel frequency.
- Returns:
  - Current frequency value.

#### `setFrequency(frequency)`

- Syntax: `setFrequency(frequency: number): boolean`
- Purpose: Change the ender item channel frequency.
- Parameters:
  - `frequency: number`
    New channel frequency.
- Returns:
  - `true` if the frequency change succeeded.
  - `false` otherwise.

#### `getChannelString()`

- Syntax: `getChannelString(): string`
- Purpose: Return the configured channel name.
- Returns:
  - Channel display string.

### energy_handler

`energy_handler` is the shared CoFH Redstone Flux storage component exposed on tiles implementing `IEnergyHandler`, `IEnergyProvider`, or `IEnergyReceiver`.

#### `getEnergyStored([direction])`

- Syntax: `getEnergyStored([direction: number]): number`
- Purpose: Return the amount of stored energy visible from a specific side.
- Parameters:
  - `direction: number`
    Optional ForgeDirection side index. If omitted, the driver uses `ForgeDirection.UNKNOWN`, which is represented as `6`.
- Returns:
  - Stored RF for that side.
- Usage notes:
  - This is the callback to use when the same machine exposes different capacities or permissions depending on side.

#### `getMaxEnergyStored([direction])`

- Syntax: `getMaxEnergyStored([direction: number]): number`
- Purpose: Return the maximum energy capacity visible from a specific side.
- Parameters:
  - `direction: number`
    Optional ForgeDirection side index, defaulting to the unknown side.
- Returns:
  - Maximum RF capacity for that side.

#### `isEnergyProvider()`

- Syntax: `isEnergyProvider(): boolean`
- Purpose: Return whether the current tile can act as an energy provider.
- Returns:
  - `true` for `IEnergyProvider` and `IEnergyHandler` tiles that expose provider behavior.
  - `false` for pure receiver-only tiles.

#### `isEnergyReceiver()`

- Syntax: `isEnergyReceiver(): boolean`
- Purpose: Return whether the current tile can act as an energy receiver.
- Returns:
  - `true` for receiver-capable tiles.
  - `false` for provider-only tiles.
- Usage notes:
  - A full `IEnergyHandler` may report both `true` and `true`.

### energy_info

`energy_info` is exposed on CoFH tiles implementing `IEnergyInfo`.

#### `getEnergy()`

- Syntax: `getEnergy(): number`
- Purpose: Return the current stored energy amount.
- Returns:
  - Current stored RF.

#### `getEnergyPerTick()`

- Syntax: `getEnergyPerTick(): number`
- Purpose: Return the machine's current energy rate.
- Returns:
  - Energy per tick as reported by the tile.
- Usage notes:
  - Depending on the target block, this may represent generation or consumption.

#### `getMaxEnergy()`

- Syntax: `getMaxEnergy(): number`
- Purpose: Return the maximum stored energy.
- Returns:
  - Maximum RF storage.

#### `getMaxEnergyPerTick()`

- Syntax: `getMaxEnergyPerTick(): number`
- Purpose: Return the maximum per-tick energy rate.
- Returns:
  - Maximum RF/t reported by the tile.

### redstone_control

`redstone_control` is exposed on CoFH tiles implementing `IRedstoneControl`.

#### `getControlDisable()`

- Syntax: `getControlDisable(): boolean`
- Purpose: Return whether the machine is in the `DISABLED` control mode.
- Returns:
  - `true` if redstone control is fully disabled.
  - `false` otherwise.

#### `getControlSetting()`

- Syntax: `getControlSetting(): number`
- Purpose: Return the current control mode as its enum ordinal.
- Returns:
  - Numeric control mode value.

#### `getControlSettingName()`

- Syntax: `getControlSettingName(): string`
- Purpose: Return the current control mode name.
- Returns:
  - Enum name string such as `DISABLED` or another CoFH control mode.

#### `getControlName(index)`

- Syntax: `getControlName(index: number): string`
- Purpose: Convert a control mode ordinal into its enum name.
- Parameters:
  - `index: number`
    Control mode ordinal.
- Returns:
  - Matching mode name string.

#### `isPowered()`

- Syntax: `isPowered(): boolean`
- Purpose: Return whether the tile is currently receiving redstone power.
- Returns:
  - `true` if powered.
  - `false` otherwise.

#### `setControlDisable()`

- Syntax: `setControlDisable(): boolean`
- Purpose: Force the machine into the `DISABLED` control mode.
- Returns:
  - Always `true` after the mode is applied.

#### `setControlSetting(state)`

- Syntax: `setControlSetting(state: number|string): boolean`
- Purpose: Change the CoFH redstone control mode.
- Parameters:
  - `state: number|string`
    Either the enum ordinal or the exact enum name.
- Returns:
  - `true` on success.
- Usage notes:
  - When using a string, the value must exactly match the underlying `ControlMode` enum name.

### secure_tile

`secure_tile` is exposed on CoFH tiles implementing `ISecurable`.

#### `canPlayerAccess(name)`

- Syntax: `canPlayerAccess(name: string): boolean`
- Purpose: Return whether the named player is allowed to access the tile.
- Parameters:
  - `name: string`
    Player name to test.
- Returns:
  - `true` if access is allowed.
  - `false` otherwise.
- Usage notes:
  - The driver resolves the player profile from the integrated or dedicated server's player list.

#### `getAccess()`

- Syntax: `getAccess(): string`
- Purpose: Return the current security access mode.
- Returns:
  - Capitalized CoFH access mode name.
- Usage notes:
  - Typical values depend on the installed CoFH version, such as `Public`, `Restricted`, or `Private`.

#### `getOwnerName()`

- Syntax: `getOwnerName(): string`
- Purpose: Return the tile owner's player name.
- Returns:
  - Owner name string.

## Example

```lua
local component = require("component")

if component.isAvailable("energy_handler") then
  local energy = component.energy_handler
  print("Stored RF:", energy.getEnergyStored())
  print("Capacity:", energy.getMaxEnergyStored())
end

if component.isAvailable("redstone_control") then
  local control = component.redstone_control
  print("Mode:", control.getControlSettingName())
  print("Powered:", control.isPowered())
end
```

## Related

- `thermalexpansion`
- `buildcraft`
