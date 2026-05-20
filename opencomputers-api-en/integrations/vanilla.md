# vanilla

## Summary

This page documents the vanilla Minecraft integrations exposed to Lua by OpenComputers. The available components cover inventories, fluid handlers, fluid tanks, command blocks, furnaces, brewing stands, note blocks, jukeboxes, beacons, comparators, and mob spawners.

## Availability

- Dependency: `vanilla`
- Label: `integration-required`

## Component Names

- `fluid_handler`
- `fluid_tank`
- `inventory`
- `command_block`
- `brewing_stand`
- `furnace`
- `beacon`
- `comparator`
- `note_block`
- `jukebox`
- `mob_spawner`

## What This Integration Is Good For

- Inspecting and rearranging ordinary inventories.
- Reading fluid tank information from vanilla-compatible tanks.
- Automating command blocks, note blocks, and jukeboxes.
- Monitoring furnace, brewing, beacon, comparator, and spawner state.

## Components

### fluid_handler

`fluid_handler` is exposed on tiles implementing `IFluidHandler`.

#### `getTankInfo([side])`

- Syntax: `getTankInfo([side: number]): table`
- Purpose: Return tank information visible from the specified side.
- Parameters:
  - `side: number`
    Optional ForgeDirection side index. Defaults to `ForgeDirection.UNKNOWN`, commonly represented as `6`.
- Returns:
  - A Lua array of fluid tank info tables.
- Usage notes:
  - This is useful for sided tanks or machines whose fill and drain behavior changes by side.

### fluid_tank

`fluid_tank` is exposed on tiles implementing `IFluidTank`.

#### `getInfo()`

- Syntax: `getInfo(): table`
- Purpose: Return information about this tank.
- Returns:
  - One fluid tank info table describing capacity and current contents.

### inventory

`inventory` is exposed on ordinary Minecraft inventories implementing `IInventory`.

#### `getInventoryName()`

- Syntax: `getInventoryName(): string[, string]`
- Purpose: Return the inventory name.
- Returns:
  - Inventory name on success.
  - `nil, "permission denied"` if the fake player permission check fails.

#### `getInventorySize()`

- Syntax: `getInventorySize(): number[, string]`
- Purpose: Return the number of slots in the inventory.
- Returns:
  - Slot count.
  - `nil, "permission denied"` if access is denied.

#### `getSlotStackSize(slot)`

- Syntax: `getSlotStackSize(slot: number): number[, string]`
- Purpose: Return how many items are currently in the specified slot.
- Parameters:
  - `slot: number`
    One-based slot index.
- Returns:
  - Current stack size.
  - `0` if the slot is empty.
  - `nil, "permission denied"` if access is denied.

#### `getSlotMaxStackSize(slot)`

- Syntax: `getSlotMaxStackSize(slot: number): number[, string]`
- Purpose: Return the maximum stack size allowed in the specified slot.
- Parameters:
  - `slot: number`
    One-based slot index.
- Returns:
  - The smaller of the inventory limit and the stack's own max size.
  - The inventory limit if the slot is empty.
  - `nil, "permission denied"` if access is denied.

#### `compareStacks(slotA, slotB)`

- Syntax: `compareStacks(slotA: number, slotB: number): boolean[, string]`
- Purpose: Compare two slots for item equality.
- Parameters:
  - `slotA: number`
    First one-based slot index.
  - `slotB: number`
    Second one-based slot index.
- Returns:
  - `true` if both slots are empty, both point to the same slot, or both hold matching items.
  - `false` otherwise.
  - `nil, "permission denied"` if access is denied.
- Usage notes:
  - The equality check compares item type and, for subtype-aware items, damage value.

#### `transferStack(slotA, slotB[, count])`

- Syntax: `transferStack(slotA: number, slotB: number[, count: number]): boolean[, string]`
- Purpose: Move, merge, or swap items between two slots in the same inventory.
- Parameters:
  - `slotA: number`
    Source one-based slot index.
  - `slotB: number`
    Destination one-based slot index.
  - `count: number`
    Optional item limit. Defaults to `64`, then clamps to the inventory stack limit.
- Returns:
  - `true` if an operation succeeded or there was nothing meaningful to do.
  - `false` if the move could not be completed.
  - `nil, "permission denied"` if access is denied.
- Usage notes:
  - Empty destination slots receive a moved stack fragment.
  - Matching stacks are merged up to available space.
  - Non-matching stacks are swapped only when `count` is at least the full size of the source stack.

#### `getStackInSlot(slot)`

- Syntax: `getStackInSlot(slot: number): table[, string]`
- Purpose: Return a full item stack description for the specified slot.
- Parameters:
  - `slot: number`
    One-based slot index.
- Returns:
  - Item stack description table on success.
  - `nil, "not enabled in config"` if item stack inspection is disabled.
  - `nil, "permission denied"` if access is denied.

#### `getAllStacks()`

- Syntax: `getAllStacks(): table[, string]`
- Purpose: Return all item stack descriptions in the inventory.
- Returns:
  - Array of item stack descriptions.
  - `nil, "not enabled in config"` if item stack inspection is disabled.
  - `nil, "permission denied"` if access is denied.

### command_block

`command_block` is exposed on vanilla command blocks.

#### `getCommand()`

- Syntax: `getCommand(): string`
- Purpose: Return the currently stored command.
- Returns:
  - Command string.

#### `setCommand(value)`

- Syntax: `setCommand(value: string): boolean`
- Purpose: Replace the command currently stored in the block.
- Parameters:
  - `value: string`
    New command text.
- Returns:
  - `true` on success.

#### `executeCommand()`

- Syntax: `executeCommand(): number[, string]`
- Purpose: Execute the stored command.
- Returns:
  - Command success count and status text.
  - `nil, "command blocks are disabled"` if the server has command blocks disabled.
- Usage notes:
  - The callback pauses briefly before execution so the command block state can update first.

### brewing_stand

`brewing_stand` is exposed on vanilla brewing stands.

#### `getBrewTime()`

- Syntax: `getBrewTime(): number`
- Purpose: Return the remaining brew time.
- Returns:
  - Remaining brewing ticks.

### furnace

`furnace` is exposed on vanilla furnaces.

#### `getBurnTime()`

- Syntax: `getBurnTime(): number`
- Purpose: Return how many ticks of burn time remain from the last consumed fuel.
- Returns:
  - Remaining burn ticks.

#### `getCookTime()`

- Syntax: `getCookTime(): number`
- Purpose: Return how long the current item has been cooking.
- Returns:
  - Current cook progress in ticks.

#### `getCurrentItemBurnTime()`

- Syntax: `getCurrentItemBurnTime(): number`
- Purpose: Return the full burn time of the fuel item currently being consumed.
- Returns:
  - Total fuel burn duration in ticks.

#### `isBurning()`

- Syntax: `isBurning(): boolean`
- Purpose: Return whether the furnace is currently active.
- Returns:
  - `true` if burning.
  - `false` otherwise.

### beacon

`beacon` is exposed on vanilla beacons.

#### `getLevels()`

- Syntax: `getLevels(): number`
- Purpose: Return the beacon pyramid level count.
- Returns:
  - Number of completed beacon levels.

#### `getPrimaryEffect()`

- Syntax: `getPrimaryEffect(): string`
- Purpose: Return the active primary potion effect name.
- Returns:
  - Potion effect name string.
  - `nil` if no valid primary effect is active.

#### `getSecondaryEffect()`

- Syntax: `getSecondaryEffect(): string`
- Purpose: Return the active secondary potion effect name.
- Returns:
  - Potion effect name string.
  - `nil` if no valid secondary effect is active.

### comparator

`comparator` is exposed on comparator tile entities.

#### `getOutputSignal()`

- Syntax: `getOutputSignal(): number`
- Purpose: Return the comparator output strength.
- Returns:
  - Current signal level.

### note_block

`note_block` is exposed on note blocks.

#### `getPitch()`

- Syntax: `getPitch(): number`
- Purpose: Return the currently configured pitch.
- Returns:
  - Pitch from `1` through `25`.

#### `setPitch(value)`

- Syntax: `setPitch(value: number): boolean`
- Purpose: Set the note block pitch.
- Parameters:
  - `value: number`
    Pitch from `1` through `25`.
- Returns:
  - `true` on success.
- Usage notes:
  - Invalid pitches raise an argument error.

#### `trigger([pitch])`

- Syntax: `trigger([pitch: number]): boolean`
- Purpose: Play the note block now, optionally changing pitch first.
- Parameters:
  - `pitch: number`
    Optional pitch from `1` through `25`.
- Returns:
  - `true` if the block had air above it and could produce sound.
  - `false` if the note block was obstructed above.
- Usage notes:
  - Even when the return value is `false`, the callback still updates the pitch if one was provided.

### jukebox

`jukebox` is exposed on jukebox tile entities.

#### `getRecord()`

- Syntax: `getRecord(): string`
- Purpose: Return the title of the inserted record.
- Returns:
  - Localized record title.
  - `nil` if no valid record is inserted.

#### `play()`

- Syntax: `play(): boolean`
- Purpose: Start playing the inserted record.
- Returns:
  - `true` if a valid record was found and playback was started.
  - `nil` if no valid record is inserted.

#### `stop()`

- Syntax: `stop()`
- Purpose: Stop playing the current record.
- Returns:
  - No value.

### mob_spawner

`mob_spawner` is exposed on vanilla mob spawners.

#### `getSpawningMobName()`

- Syntax: `getSpawningMobName(): string`
- Purpose: Return the entity id currently configured in the spawner.
- Returns:
  - Entity name string.

## Example

```lua
local component = require("component")

if component.isAvailable("inventory") then
  local inv = component.inventory
  print("Inventory:", inv.getInventoryName(), inv.getInventorySize())
end

if component.isAvailable("furnace") then
  local furnace = component.furnace
  print("Burning:", furnace.isBurning(), furnace.getCookTime())
end

if component.isAvailable("note_block") then
  local note = component.note_block
  note.setPitch(13)
  note.trigger()
end
```

## Related

- `redstone`
- `cofh`
