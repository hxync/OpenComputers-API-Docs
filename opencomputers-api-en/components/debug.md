# debug

## Summary

The `debug` component is the Lua API exposed by the OpenComputers debug card. It is a creative-oriented, highly privileged tool for world inspection, world editing, command execution, player manipulation, inventory and fluid access, and inter-card messaging.

This component is powerful enough to bypass normal survival gameplay limits. Most callbacks first verify the card's access context, so an unbound or unauthorized card will raise an access error before doing any work.

## Availability

- Repository: `opencomputers`
- Component name: `debug`
- Typical host: computer or robot with a debug card installed

## Important Notes

- Bind the card to yourself by sneak-using it if you want command execution and access checks to use your permissions.
- Methods returning `PlayerValue` or `WorldValue` create wrapper userdata objects. The real work happens on those objects.
- `sendToDebugCard` delivers a `debug_message` signal to the destination machine.

## Usage

```lua
local component = require("component")
local debug = component.debug

if not debug then
  error("debug card not installed")
end
```

## Card-Level API

### changeBuffer

- Syntax: `debug.changeBuffer(value)`
- Returns: `number`
- Purpose: Adds the specified delta to the component network's energy buffer.

Parameter:

- `value:number`: positive to charge the buffer, negative to drain it.

Example:

```lua
print("New buffer level:", debug.changeBuffer(2500))
```

### getX

- Syntax: `debug.getX()`
- Returns: `number`
- Purpose: Gets the host container's X coordinate.

### getY

- Syntax: `debug.getY()`
- Returns: `number`
- Purpose: Gets the host container's Y coordinate.

### getZ

- Syntax: `debug.getZ()`
- Returns: `number`
- Purpose: Gets the host container's Z coordinate.

Example:

```lua
print("Debug card location:", debug.getX(), debug.getY(), debug.getZ())
```

### getWorld

- Syntax: `debug.getWorld([dimensionId])`
- Returns: `WorldValue`
- Purpose: Gets a world wrapper for the specified dimension, or for the host world when omitted.

Parameter:

- `dimensionId:number` optional: dimension ID to wrap.

Use `getWorlds()` first if you need to enumerate dimensions. Passing an unloaded dimension ID can produce a wrapper around a missing world, so practical scripts usually target loaded worlds.

Example:

```lua
local world = debug.getWorld()
print("Dimension:", world.getDimensionName())
```

### getWorlds

- Syntax: `debug.getWorlds()`
- Returns: `table`
- Purpose: Returns all registered dimension IDs, including loaded and unloaded ones.

Example:

```lua
for _, id in ipairs(debug.getWorlds()) do
  print("Dimension ID:", id)
end
```

### getPlayer

- Syntax: `debug.getPlayer(name)`
- Returns: `PlayerValue`
- Purpose: Gets a player wrapper object by player name.

Parameter:

- `name:string`: exact player name.

The wrapper is created even if the player is currently offline. Offline checks happen when you call methods on the `PlayerValue`.

Example:

```lua
local steve = debug.getPlayer("Steve")
print(steve.getHealth())
```

### getPlayers

- Syntax: `debug.getPlayers()`
- Returns: `table`
- Purpose: Lists players currently logged in to the server.

Example:

```lua
for _, name in ipairs(debug.getPlayers()) do
  print("Online:", name)
end
```

### scanContentsAt

- Syntax: `debug.scanContentsAt(x, y, z[, worldId])`
- Returns: `boolean, string, value`
- Purpose: Inspects what exists at a position, optionally in another loaded world.

Parameters:

- `x:number`, `y:number`, `z:number`: absolute block coordinates.
- `worldId:number` optional: dimension ID. When omitted, the host world is used.

Return value meaning:

- Second result identifies the category:
  - `"EntityLivingBase"`
  - `"EntityMinecart"`
  - `"air"`
  - `"liquid"`
  - `"replaceable"`
  - `"passable"`
  - `"solid"`
- Third result is the matching entity or block object.
- First result is a status flag:
  - `false` for air,
  - `true` for entities, passable blocks, and solid blocks,
  - for liquids and replaceable blocks it mirrors whether a fake player break event was canceled at that location.

Example:

```lua
local ok, kind, value = debug.scanContentsAt(0, 64, 0)
print("Kind:", kind, "Flag:", ok)
print("Value:", value)
```

### isModLoaded

- Syntax: `debug.isModLoaded(name)`
- Returns: `boolean`
- Purpose: Checks whether a Forge mod or registered API is available.

Parameter:

- `name:string`: mod ID or API name.

Example:

```lua
if debug.isModLoaded("gregtech") then
  print("GregTech is available")
end
```

### runCommand

- Syntax:
  - `debug.runCommand(command)`
  - `debug.runCommand(commandsTable)`
- Returns: `number, string|nil`
- Purpose: Executes one command or a list of commands through the debug card's fake player command sender.

Parameters:

- `command:string`: a single command line.
- `commandsTable:table`: table of command strings to execute in sequence.

Return values:

- First result: numeric return value from the last executed command.
- Second result: concatenated chat/output messages captured while running the command(s), or `nil` when there was no output.

Example:

```lua
local count, output = debug.runCommand("/time set day")
print("Affected:", count)
if output then
  print(output)
end
```

### connectToBlock

- Syntax: `debug.connectToBlock(x, y, z)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Connects a component block at the given coordinates into the current computer network.

Parameters:

- `x:number`, `y:number`, `z:number`: absolute coordinates of the target block.

Behavior:

- If the card was previously connected to another remote node, that connection is removed first.
- It can attach to regular `Environment` tiles and sided environments.
- Failure reason from source: `"no node found at this position"`.

Example:

```lua
local ok, reason = debug.connectToBlock(10, 64, -5)
if not ok then
  io.stderr:write("Connect failed: " .. tostring(reason) .. "\n")
end
```

### test

- Syntax: `debug.test()`
- Returns: multiple sample values
- Purpose: Internal test callback for userdata and value conversion.

It mainly exists for development and compatibility testing, not normal gameplay scripts.

### sendToClipboard

- Syntax: `debug.sendToClipboard(player, text)`
- Returns: `boolean[, string]`
- Purpose: Sends text to a player's client clipboard when the target player is online.

Parameters:

- `player:string`: player name.
- `text:string`: clipboard text.

Failure reason from source: `"no such player"`.

Example:

```lua
local ok, reason = debug.sendToClipboard("Steve", "Coordinates copied")
if not ok then
  print(reason)
end
```

### sendToDebugCard

- Syntax: `debug.sendToDebugCard(address, data...)`
- Returns: no direct return values
- Purpose: Sends arbitrary payload data to another debug card by component address.

Parameters:

- `address:string`: destination debug card address.
- `data...`: values to include in the packet payload.

On the destination machine, the packet becomes a `computer.signal` event:

```lua
"debug_message", sourceAddress, port, distance, ...
```

Example:

```lua
debug.sendToDebugCard("01234567-89ab-cdef-0123-456789abcdef", "sync", 42, true)
```

## PlayerValue API

`debug.getPlayer(name)` returns a `PlayerValue`. All methods below fail with `nil, "player is offline"` when the named player is not currently online.

### getWorld

- Syntax: `playerValue.getWorld()`
- Returns: `WorldValue`
- Purpose: Gets the world the player is currently in.

### getGameType

- Syntax: `playerValue.getGameType()`
- Returns: `string`
- Purpose: Gets the player's game mode name such as `survival`, `creative`, or `adventure`.

### setGameType

- Syntax: `playerValue.setGameType(gametype)`
- Returns: no direct return values
- Purpose: Changes the player's game mode.

Parameter:

- `gametype:string`: game mode name. Unknown names fall back to `survival` in source.

### getPosition

- Syntax: `playerValue.getPosition()`
- Returns: `number, number, number`
- Purpose: Gets the player's current position.

### setPosition

- Syntax: `playerValue.setPosition(x, y, z)`
- Returns: no direct return values
- Purpose: Teleports the player to the specified coordinates.

### getHealth

- Syntax: `playerValue.getHealth()`
- Returns: `number`
- Purpose: Gets the player's current health.

### getMaxHealth

- Syntax: `playerValue.getMaxHealth()`
- Returns: `number`
- Purpose: Gets the player's maximum health.

### setHealth

- Syntax: `playerValue.setHealth(health)`
- Returns: no direct return values
- Purpose: Sets the player's current health value directly.

Example:

```lua
local player = debug.getPlayer("Steve")
local x, y, z = player.getPosition()
print("Player position:", x, y, z)
player.setGameType("creative")
```

## WorldValue API

`debug.getWorld(...)` and `playerValue.getWorld()` return a `WorldValue`.

### World State

#### getDimensionId

- Syntax: `world.getDimensionId()`
- Returns: `number`
- Purpose: Gets the numeric dimension ID.

#### getDimensionName

- Syntax: `world.getDimensionName()`
- Returns: `string`
- Purpose: Gets the dimension name.

#### getSeed

- Syntax: `world.getSeed()`
- Returns: `number`
- Purpose: Gets the world seed.

#### isRaining

- Syntax: `world.isRaining()`
- Returns: `boolean`
- Purpose: Checks whether it is currently raining.

#### setRaining

- Syntax: `world.setRaining(value)`
- Returns: no direct return values
- Purpose: Enables or disables rain.

#### isThundering

- Syntax: `world.isThundering()`
- Returns: `boolean`
- Purpose: Checks whether it is currently thundering.

#### setThundering

- Syntax: `world.setThundering(value)`
- Returns: no direct return values
- Purpose: Enables or disables thunder.

#### getTime

- Syntax: `world.getTime()`
- Returns: `number`
- Purpose: Gets the current world time.

#### setTime

- Syntax: `world.setTime(value)`
- Returns: no direct return values
- Purpose: Sets the current world time.

#### getSpawnPoint

- Syntax: `world.getSpawnPoint()`
- Returns: `number, number, number`
- Purpose: Gets the world spawn position.

#### setSpawnPoint

- Syntax: `world.setSpawnPoint(x, y, z)`
- Returns: no direct return values
- Purpose: Changes the world spawn position.

#### playSoundAt

- Syntax: `world.playSoundAt(x, y, z, sound, range)`
- Returns: no direct return values
- Purpose: Plays a sound effect at the specified coordinates.

Parameters:

- `sound:string`: internal sound name.
- `range:number`: audible range scaling value used by the source implementation.

### Block and Tile Inspection

#### getBlockId

- Syntax: `world.getBlockId(x, y, z)`
- Returns: `number`
- Purpose: Gets the numeric block ID at the coordinates.

#### getMetadata

- Syntax: `world.getMetadata(x, y, z)`
- Returns: `number`
- Purpose: Gets the block metadata.

#### isLoaded

- Syntax: `world.isLoaded(x, y, z)`
- Returns: `boolean`
- Purpose: Checks whether the block coordinates are currently loaded.

#### hasTileEntity

- Syntax: `world.hasTileEntity(x, y, z)`
- Returns: `boolean`
- Purpose: Checks whether the block at the coordinates provides a tile entity.

#### getTileNBT

- Syntax: `world.getTileNBT(x, y, z)`
- Returns: `table` or `nil`
- Purpose: Serializes the tile entity at the position into an NBT-like Lua table.

If no tile entity exists, the call returns `nil`.

#### setTileNBT

- Syntax: `world.setTileNBT(x, y, z, nbt)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Replaces a tile entity's NBT data from a Lua table.

Failure reasons from source:

- `"no tile entity"`
- `"nbt tag compound expected, got '<type>'"`

#### getLightOpacity

- Syntax: `world.getLightOpacity(x, y, z)`
- Returns: `number`
- Purpose: Gets the block's light opacity value.

#### getLightValue

- Syntax: `world.getLightValue(x, y, z)`
- Returns: `number`
- Purpose: Gets the block's emitted light value.

#### canSeeSky

- Syntax: `world.canSeeSky(x, y, z)`
- Returns: `boolean`
- Purpose: Checks whether the coordinates are directly open to the sky.

### Block Editing

#### setBlock

- Syntax: `world.setBlock(x, y, z, id, meta)`
- Returns: `boolean`
- Purpose: Sets a single block at the specified coordinates.

Parameters:

- `id:number|string`: numeric block ID or registered block name.
- `meta:number`: metadata value to place.

#### setBlocks

- Syntax: `world.setBlocks(x1, y1, z1, x2, y2, z2, id, meta)`
- Returns: no direct return values
- Purpose: Fills the entire axis-aligned area between two corners with the specified block.

The source iterates inclusively across all coordinates between the two points.

### Inventory and Fluid Access

#### insertItem

- Syntax: `world.insertItem(id, count, damage, nbtJson, x, y, z, side)`
- Returns:
  - Success: `true`
  - Failure: `false` or `nil, reason`
- Purpose: Inserts an item stack into the inventory at the specified position.

Parameters:

- `id:string`: registry item ID.
- `count:number`: desired item count.
- `damage:number`: damage / metadata value.
- `nbtJson:string`: optional NBT data encoded as JSON. Use `""` for none.
- `x:number`, `y:number`, `z:number`: target block coordinates.
- `side:number`: insertion side.

Behavior:

- Invalid item IDs raise `"invalid item id"`.
- If there is no inventory, the call returns `nil, "no inventory"`.
- If insertion is possible, the code keeps trying until the requested amount is inserted or no more progress can be made.

#### removeItem

- Syntax: `world.removeItem(x, y, z, slot[, count])`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Removes items from the specified inventory slot and returns how many items were actually removed.

Failure reason from source: `"no inventory"`.

#### insertFluid

- Syntax: `world.insertFluid(id, amount, x, y, z, side)`
- Returns:
  - Success: `number`
  - Failure: `nil, reason`
- Purpose: Inserts fluid into a tank and returns the amount that was accepted.

Behavior:

- Invalid fluid IDs raise `"invalid fluid id"`.
- If there is no fluid handler, the call returns `nil, "no tank"`.

#### removeFluid

- Syntax: `world.removeFluid(amount, x, y, z, side)`
- Returns:
  - Success: drained fluid stack data
  - Failure: `nil, reason`
- Purpose: Drains fluid from a tank on the specified side.

Failure reason from source: `"no tank"`.

Example:

```lua
local world = debug.getWorld()
print("Seed:", world.getSeed())
print("Spawn:", world.getSpawnPoint())

local ok, reason = world.setTileNBT(0, 64, 0, {energy = 5000})
if ok == nil then
  print("NBT write failed:", reason)
end
```

## Related

- `component`
- `computer`
- `linked card`
