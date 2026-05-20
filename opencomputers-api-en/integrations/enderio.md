# enderio

## Summary

This page documents the Ender IO integrations exposed through OpenComputers and Computronics. The integration covers machine state, capacitor banks, XP storage tiles, configurable IO sides, power monitors, generic power storage, redstone control, telepads, dimensional transceivers, vacuum chests, and weather obelisks.

## Availability

- Dependency: `enderio`
- Label: `integration-required`

## Component Names

- `enderio_machine`
- `capacitor_bank`
- `experience_tile`
- `io_config_tile`
- `power_monitor`
- `power_storage`
- `redstone_tile`
- `telepad`
- `dimensional_transceiver`
- `vacuum_chest`
- `weather_obelisk`

## What This Integration Is Good For

- Monitoring whether Ender IO machines are active and how fast they are progressing.
- Tuning capacitor-bank throughput and redstone gating.
- Reading or changing per-side IO configuration.
- Configuring telepad destinations from Lua.
- Automating dimensional transceiver channel membership.
- Monitoring power networks, XP storage, vacuum range, and weather devices.

## Components

### enderio_machine

`enderio_machine` is a shared component family. A particular machine may expose only part of this API, depending on which Ender IO interfaces it implements.

#### isActive()

- Syntax: `isActive(): boolean`
- Purpose: Return whether the machine is currently active.
- Usage notes:
  - This callback comes from the `AbstractMachineEntity` driver.

#### getPowerPerTick()

- Syntax: `getPowerPerTick(): number`
- Purpose: Return the machine's per-tick power use value from `getPowerUsePerTick()`.
- Usage notes:
  - This callback comes from the `AbstractPoweredMachineEntity` driver.

#### getProgress()

- Syntax: `getProgress(): number`
- Purpose: Return the machine's internal progress value.
- Usage notes:
  - This callback comes from the `IProgressTile` driver.
  - The returned value is whatever Ender IO stores internally. It is not guaranteed to be a normalized percentage.

### capacitor_bank

`capacitor_bank` is exposed on Ender IO capacitor banks.

#### getAverageInputPerTick()

- Syntax: `getAverageInputPerTick(): number`
- Purpose: Return the recent average energy input per tick.
- Returns:
  - The network average when the bank belongs to a capacitor-bank network.
  - `0` when no bank network object is available.

#### getAverageOutputPerTick()

- Syntax: `getAverageOutputPerTick(): number`
- Purpose: Return the recent average energy output per tick.
- Returns:
  - The network average when the bank belongs to a capacitor-bank network.
  - `0` when no bank network object is available.

#### getAverageChangePerTick()

- Syntax: `getAverageChangePerTick(): number`
- Purpose: Return the recent average net storage change per tick.
- Returns:
  - The network average when the bank belongs to a capacitor-bank network.
  - `0` when no bank network object is available.

#### setMaxInput(max)

- Syntax: `setMaxInput(max: number)`
- Purpose: Change the maximum accepted input rate.
- Parameters:
  - `max`: new maximum input value.
- Returns:
  - No useful return value on success.
- Usage notes:
  - When the bank is part of a network, the write is applied to the network object instead of the single block.

#### setMaxOutput(max)

- Syntax: `setMaxOutput(max: number)`
- Purpose: Change the maximum output rate.
- Parameters:
  - `max`: new maximum output value.
- Returns:
  - No useful return value on success.
- Usage notes:
  - When the bank is part of a network, the write is applied to the network object instead of the single block.

#### getInputMode()

- Syntax: `getInputMode(): string`
- Purpose: Return the current redstone control mode for input.

#### getOutputMode()

- Syntax: `getOutputMode(): string`
- Purpose: Return the current redstone control mode for output.

#### setInputMode(mode)

- Syntax: `setInputMode(mode: string)`
- Purpose: Change the redstone control mode for input.
- Parameters:
  - `mode`: one of `ignore`, `on`, `off`, `never`.
- Returns:
  - No useful return value on success.
- Usage notes:
  - Invalid values raise `No valid Redstone mode given`.

#### setOutputMode(mode)

- Syntax: `setOutputMode(mode: string)`
- Purpose: Change the redstone control mode for output.
- Parameters:
  - `mode`: one of `ignore`, `on`, `off`, `never`.
- Returns:
  - No useful return value on success.
- Usage notes:
  - Invalid values raise `No valid Redstone mode given`.

#### redstone_modes

- Syntax: `redstone_modes`
- Purpose: Return the list of supported redstone control modes.
- Returns:
  - An indexed Lua table containing `ignore`, `on`, `off`, and `never`.

### experience_tile

`experience_tile` is exposed on Ender IO tiles implementing `IHaveExperience`.

#### getExperience()

- Syntax: `getExperience(): number`
- Purpose: Return the total experience stored in the tile's XP container.

#### getExperienceLevels()

- Syntax: `getExperienceLevels(): number`
- Purpose: Return the current represented experience level count.

#### getMaxExperience()

- Syntax: `getMaxExperience(): number`
- Purpose: Return the maximum XP the tile can store.

### io_config_tile

`io_config_tile` is exposed on Ender IO machines implementing per-side IO configuration.

#### getIOMode(side)

- Syntax: `getIOMode(side: number): string`
- Purpose: Return the current IO mode of one side.
- Parameters:
  - `side`: side index from `1` to `6`.
- Returns:
  - One of the lower-case IO mode names.
- Usage notes:
  - The driver converts the Lua side value to zero-based `ForgeDirection`.
  - Values outside `1..6` raise `side needs to be between 1 and 6`.

#### setIOMode(side, mode)

- Syntax: `setIOMode(side: number, mode: string)`
- Purpose: Change the IO mode of one side.
- Parameters:
  - `side`: side index from `1` to `6`.
  - `mode`: one of `none`, `pull`, `push`, `push_pull`, `disabled`.
- Returns:
  - No useful return value on success.
- Usage notes:
  - Invalid side values raise `side needs to be between 1 and 6`.
  - Invalid mode values raise `No valid IO mode given`.

#### io_modes

- Syntax: `io_modes`
- Purpose: Return the list of supported IO modes.
- Returns:
  - An indexed Lua table containing `none`, `pull`, `push`, `push_pull`, and `disabled`.

### power_monitor

`power_monitor` is exposed on Ender IO power monitor blocks.

#### getPowerInConduits()

- Syntax: `getPowerInConduits(): number`
- Purpose: Return the energy currently stored in the conduit network.

#### getMaxPowerInConduits()

- Syntax: `getMaxPowerInConduits(): number`
- Purpose: Return the maximum energy the conduit network can store.

#### getPowerInCapBanks()

- Syntax: `getPowerInCapBanks(): number`
- Purpose: Return the energy currently stored in connected capacitor banks.

#### getMaxPowerInCapBanks()

- Syntax: `getMaxPowerInCapBanks(): number`
- Purpose: Return the maximum energy connected capacitor banks can store.

#### getPowerInMachines()

- Syntax: `getPowerInMachines(): number`
- Purpose: Return the energy currently stored in connected machines.

#### getMaxPowerInMachines()

- Syntax: `getMaxPowerInMachines(): number`
- Purpose: Return the maximum energy connected machines can store.

#### getAverageEnergySent()

- Syntax: `getAverageEnergySent(): number`
- Purpose: Return the recent average energy sent value.

#### getAverageEnergyReceived()

- Syntax: `getAverageEnergyReceived(): number`
- Purpose: Return the recent average energy received value.

#### isEngineControlEnabled()

- Syntax: `isEngineControlEnabled(): boolean`
- Purpose: Return whether Engine Control is enabled.

#### setEngineControlEnabled(control)

- Syntax: `setEngineControlEnabled(control: boolean)`
- Purpose: Enable or disable Engine Control.
- Parameters:
  - `control`: desired Engine Control state.
- Returns:
  - No useful return value on success.
- Usage notes:
  - The ComputerCraft-side error string mistakenly says `first argument needs to be a number`, but the actual required type is still boolean.

#### getStartLevel()

- Syntax: `getStartLevel(): number`
- Purpose: Return the level at which the monitor starts emitting redstone.

#### setStartLevel(level)

- Syntax: `setStartLevel(level: number)`
- Purpose: Change the level at which the monitor starts emitting redstone.
- Parameters:
  - `level`: new threshold value.
- Returns:
  - No useful return value on success.
- Usage notes:
  - The driver stores the number as a float.

#### getStopLevel()

- Syntax: `getStopLevel(): number`
- Purpose: Return the level at which the monitor stops emitting redstone.

#### setStopLevel(level)

- Syntax: `setStopLevel(level: number)`
- Purpose: Change the level at which the monitor stops emitting redstone.
- Parameters:
  - `level`: new threshold value.
- Returns:
  - No useful return value on success.
- Usage notes:
  - The driver stores the number as a float.

### power_storage

`power_storage` is exposed on generic Ender IO power-storage tiles implementing `IPowerStorage`.

#### getMaxInput()

- Syntax: `getMaxInput(): number`
- Purpose: Return the maximum accepted input rate.

#### getMaxOutput()

- Syntax: `getMaxOutput(): number`
- Purpose: Return the maximum output rate.

#### getEnergyStored()

- Syntax: `getEnergyStored(): number`
- Purpose: Return the currently stored energy amount.

#### getMaxEnergyStored()

- Syntax: `getMaxEnergyStored(): number`
- Purpose: Return the maximum stored energy capacity.

#### Usage notes

- These callbacks are read-only.
- The OC driver class internally names its environment like a capacitor-bank component in one path, but the exposed component family documented here is the generic power-storage API.

### redstone_tile

`redstone_tile` is exposed on Ender IO tiles implementing generic redstone mode control.

#### getRedstoneMode()

- Syntax: `getRedstoneMode(): string`
- Purpose: Return the current redstone control mode.

#### setRedstoneMode(mode)

- Syntax: `setRedstoneMode(mode: string)`
- Purpose: Change the redstone control mode.
- Parameters:
  - `mode`: one of `ignore`, `on`, `off`, `never`.
- Returns:
  - No useful return value on success.
- Usage notes:
  - Invalid values raise `No valid Redstone mode given`.

#### redstone_modes

- Syntax: `redstone_modes`
- Purpose: Return the list of supported redstone modes.
- Returns:
  - An indexed Lua table containing `ignore`, `on`, `off`, and `never`.

### telepad

`telepad` is exposed on Ender IO telepads.

#### getX()

- Syntax: `getX(): number`
- Purpose: Return the configured target X coordinate.
- Usage notes:
  - The callback requires the telepad to be part of a valid telepad network and otherwise raises `telepad is not a valid structure`.

#### getY()

- Syntax: `getY(): number`
- Purpose: Return the configured target Y coordinate.
- Usage notes:
  - The callback requires a valid telepad structure.

#### getZ()

- Syntax: `getZ(): number`
- Purpose: Return the configured target Z coordinate.
- Usage notes:
  - The callback requires a valid telepad structure.

#### getCoords()

- Syntax: `getCoords(): number, number, number`
- Purpose: Return the configured target coordinates.
- Usage notes:
  - The callback requires a valid telepad structure.

#### getDimension()

- Syntax: `getDimension(): number`
- Purpose: Return the configured target dimension ID.
- Usage notes:
  - The callback requires a valid telepad structure.

#### setX(x)

- Syntax: `setX(x: number): number | nil, string?`
- Purpose: Change the target X coordinate.
- Parameters:
  - `x`: new target X coordinate.
- Returns:
  - The new X coordinate on success.
  - `nil, "not enabled in config"` when coordinate editing is disabled by config.
- Usage notes:
  - The callback requires a valid telepad structure.

#### setY(y)

- Syntax: `setY(y: number): number | nil, string?`
- Purpose: Change the target Y coordinate.
- Parameters:
  - `y`: new target Y coordinate.
- Returns:
  - The new Y coordinate on success.
  - `nil, "not enabled in config"` when coordinate editing is disabled by config.
- Usage notes:
  - The callback requires a valid telepad structure.

#### setZ(z)

- Syntax: `setZ(z: number): number | nil, string?`
- Purpose: Change the target Z coordinate.
- Parameters:
  - `z`: new target Z coordinate.
- Returns:
  - The new Z coordinate on success.
  - `nil, "not enabled in config"` when coordinate editing is disabled by config.
- Usage notes:
  - The callback requires a valid telepad structure.

#### setCoords(x, y, z)

- Syntax: `setCoords(x: number, y: number, z: number): number, number, number | nil, string?`
- Purpose: Change all target coordinates in one call.
- Parameters:
  - `x`, `y`, `z`: new target coordinates.
- Returns:
  - The new `x, y, z` on success.
  - `nil, "not enabled in config"` when coordinate editing is disabled by config.
- Usage notes:
  - The callback requires a valid telepad structure.

#### setDimension(dimension)

- Syntax: `setDimension(dimension: number): number | nil, string?`
- Purpose: Change the target dimension ID.
- Parameters:
  - `dimension`: new dimension ID.
- Returns:
  - The new dimension ID on success.
  - `nil, "not enabled in config"` when dimension editing is disabled by config.
- Usage notes:
  - The callback requires a valid telepad structure.

#### teleport()

- Syntax: `teleport()`
- Purpose: Activate the telepad and teleport valid entities.
- Returns:
  - No useful return value on success.
- Usage notes:
  - The OC driver checks that the telepad is a valid structure before teleporting and otherwise raises `telepad is not a valid structure`.

#### isValid()

- Syntax: `isValid(): boolean`
- Purpose: Return whether the telepad is currently part of a valid telepad network.

### dimensional_transceiver

`dimensional_transceiver` is exposed on Ender IO transceivers.

#### getSendChannels(channelType)

- Syntax: `getSendChannels(channelType: string): table`
- Purpose: Return the channels this transceiver currently sends to for the specified type.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
- Returns:
  - An indexed Lua table of channel names.
- Usage notes:
  - Invalid channel types raise `No valid channel type given`.

#### setSendChannel(channelType, name, shouldSend)

- Syntax: `setSendChannel(channelType: string, name: string, shouldSend: boolean): boolean`
- Purpose: Add or remove one send channel from the local transceiver configuration.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
  - `name`: channel name.
  - `shouldSend`: `true` to add the channel, `false` to remove it.
- Returns:
  - `true` when the requested change was applied.
  - `false` when the named channel could not be found in the relevant source list.

#### getReceiveChannels(channelType)

- Syntax: `getReceiveChannels(channelType: string): table`
- Purpose: Return the channels this transceiver currently receives from for the specified type.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
- Returns:
  - An indexed Lua table of channel names.
- Usage notes:
  - Invalid channel types raise `No valid channel type given`.

#### setReceiveChannel(channelType, name, shouldReceive)

- Syntax: `setReceiveChannel(channelType: string, name: string, shouldReceive: boolean): boolean`
- Purpose: Add or remove one receive channel from the local transceiver configuration.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
  - `name`: channel name.
  - `shouldReceive`: `true` to add the channel, `false` to remove it.
- Returns:
  - `true` when the requested change was applied.
  - `false` when the named channel could not be found in the relevant source list.

#### addChannel(channelType, name)

- Syntax: `addChannel(channelType: string, name: string): boolean`
- Purpose: Add a public channel to the global registry.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
  - `name`: channel name.
- Returns:
  - `true` when the channel was created.
  - `false` when a channel with the same name already exists for that type, or the type cannot be resolved.

#### removeChannel(channelType, name)

- Syntax: `removeChannel(channelType: string, name: string): boolean`
- Purpose: Remove a public channel from the global registry.
- Parameters:
  - `channelType`: one of `power`, `item`, `fluid`, `rail`.
  - `name`: channel name.
- Returns:
  - `true` when the channel was removed.
  - `false` when no matching channel exists.

#### isChannelExisting(channelType, name)

- Syntax: `isChannelExisting(channelType: string, name: string): boolean`
- Purpose: Return whether a public channel currently exists.

#### getChannels(channelType)

- Syntax: `getChannels(channelType: string): table`
- Purpose: Return every public channel registered for the specified type.
- Returns:
  - An indexed Lua table of channel names.

#### isSendChannel(channelType, name)

- Syntax: `isSendChannel(channelType: string, name: string): boolean`
- Purpose: Return whether this transceiver is configured to send to the specified channel.

#### isReceiveChannel(channelType, name)

- Syntax: `isReceiveChannel(channelType: string, name: string): boolean`
- Purpose: Return whether this transceiver is configured to receive from the specified channel.

#### channel_types

- Syntax: `channel_types`
- Purpose: Return the list of available channel types.
- Returns:
  - An indexed Lua table containing `power`, `item`, `fluid`, and `rail`.

### vacuum_chest

`vacuum_chest` is exposed on Ender IO vacuum chests.

#### getRange()

- Syntax: `getRange(): number`
- Purpose: Return the current vacuum range.

#### setRange(range)

- Syntax: `setRange(range: number)`
- Purpose: Change the vacuum range.
- Parameters:
  - `range`: new range value.
- Returns:
  - No useful return value on success.

### weather_obelisk

`weather_obelisk` is exposed on Ender IO weather obelisks.

#### canActivate(task)

- Syntax: `canActivate(task: number): boolean[, string]`
- Purpose: Return whether the specified weather task can currently start.
- Parameters:
  - `task`: numeric weather mode ID.
- Returns:
  - `true` or `false` for valid mode IDs.
  - `false, "invalid weather mode. needs to be between 1 and N"` for out-of-range IDs.
- Usage notes:
  - The supported modes are exposed through `weather_modes`.

#### activate()

- Syntax: `activate(): boolean`
- Purpose: Attempt to start a weather task immediately.
- Returns:
  - `true` when a task starts successfully.
  - `false` when a task is already active or the current prerequisites are not met.
- Usage notes:
  - The obelisk needs the required fluid, a firework, no active task, and no cooldown before it can start a task.

#### weather_modes

- Syntax: `weather_modes`
- Purpose: Return the mapping of weather mode names to numeric IDs.
- Returns:
  - A Lua table containing:
    - `clear = 1`
    - `rain = 2`
    - `storm = 3`

## Example

```lua
local component = require("component")

if component.isAvailable("capacitor_bank") then
  local bank = component.capacitor_bank
  print("Cap bank input mode:", bank.getInputMode())
  print("Average change:", bank.getAverageChangePerTick())
end

if component.isAvailable("telepad") then
  local telepad = component.telepad
  if telepad.isValid() then
    local x, y, z = telepad.getCoords()
    print("Telepad target:", x, y, z, "dim", telepad.getDimension())
  end
end
```

## Related

- `opencomputers-redstone-vanilla`
- `computronics`
