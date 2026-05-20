# opencomputers-tps-card

## Summary

This page documents the `tps_card` component. It exposes server and dimension performance statistics such as average tick time, loaded entity counts, loaded chunk counts, and per-dimension breakdowns.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `tps_card`
- Backing implementation: `li.cil.oc.server.component.TpsCard`
- Typical host: TPS card upgrade or host exposing that upgrade

## What It Measures

- Average server tick time in milliseconds.
- Average tick time per loaded dimension.
- Number of loaded dimensions, chunks, tile entities, and entities.
- Per-dimension entity and tile entity class counts.
- Coordinates of entities of a specific Java class, when access is permitted.

## Dimension Handling

Some callbacks operate only on loaded dimensions.

- If the requested dimension is not loaded, those callbacks return `nil, "Dimension not loaded: <id>"`.
- `getTickTimeInDim()` uses the host's current dimension when you omit the argument.

## Class Name Handling

Callbacks that list or search entities use full Java class names, for example:

- `net.minecraft.entity.item.EntityItem`
- `net.minecraft.entity.monster.EntityZombie`

Use `getEntitiesListForDim()` first if you need the exact class name to pass into `getCoordinatesForEntityClassInDim()`.

## API

### getTickTimeInDim

- Syntax: `tpsCard.getTickTimeInDim([dimension])`
- Parameters:
  - `dimension:number`: dimension id. Defaults to the host's current dimension.
- Returns: `number`
- Purpose: Returns the average tick time of one dimension in milliseconds.

Lower values are better. Around `50` ms corresponds to `20 TPS`.

### getOverallTickTime

- Syntax: `tpsCard.getOverallTickTime()`
- Returns: `number`
- Purpose: Returns the average overall server tick time in milliseconds.

### getAllDims

- Syntax: `tpsCard.getAllDims()`
- Returns: `table`
- Purpose: Returns a table mapping each loaded dimension id to its dimension name.

### getAllTickTimes

- Syntax: `tpsCard.getAllTickTimes()`
- Returns: `table`
- Purpose: Returns a table mapping each loaded dimension id to its average tick time in milliseconds.

### getNameForDim

- Syntax: `tpsCard.getNameForDim(dimension)`
- Parameters:
  - `dimension:number`: dimension id.
- Returns:
  - Success: `string`
  - Failure: `nil, "Dimension not loaded: <id>"`
- Purpose: Returns the display name of a loaded dimension.

### convertTickTimeIntoTps

- Syntax: `tpsCard.convertTickTimeIntoTps(time)`
- Parameters:
  - `time:number`: tick time in milliseconds.
- Returns: `number`
- Purpose: Converts a tick time in milliseconds into a TPS value.

Behavior details:

- The result is capped at `20`.
- Values below `50` ms therefore also report at most `20`.
- This helper does not validate that the input is positive.

### getOverallTileEntitiesLoaded

- Syntax: `tpsCard.getOverallTileEntitiesLoaded()`
- Returns: `number`
- Purpose: Returns the total number of loaded tile entities across all loaded dimensions.

### getOverallChunksLoaded

- Syntax: `tpsCard.getOverallChunksLoaded()`
- Returns: `number`
- Purpose: Returns the total number of loaded chunks across all loaded dimensions.

### getOverallEntitiesLoaded

- Syntax: `tpsCard.getOverallEntitiesLoaded()`
- Returns: `number`
- Purpose: Returns the total number of loaded entities across all loaded dimensions.

### getOverallDimsLoaded

- Syntax: `tpsCard.getOverallDimsLoaded()`
- Returns: `number`
- Purpose: Returns how many dimensions are currently loaded on the server.

### getEntitiesListForDim

- Syntax: `tpsCard.getEntitiesListForDim(dimension)`
- Parameters:
  - `dimension:number`: dimension id.
- Returns:
  - Success: `table`
  - Failure: `nil, "Dimension not loaded: <id>"`
- Purpose: Returns a table mapping full entity class names to the number of loaded entities of that class in the dimension.

### getTileEntitiesListForDim

- Syntax: `tpsCard.getTileEntitiesListForDim(dimension)`
- Parameters:
  - `dimension:number`: dimension id.
- Returns:
  - Success: `table`
  - Failure: `nil, "Dimension not loaded: <id>"`
- Purpose: Returns a table mapping full tile entity class names to the number of loaded tile entities of that class in the dimension.

### getChunksLoadedForDim

- Syntax: `tpsCard.getChunksLoadedForDim(dimension)`
- Parameters:
  - `dimension:number`: dimension id.
- Returns:
  - Success: `number`
  - Failure: `nil, "Dimension not loaded: <id>"`
- Purpose: Returns the number of loaded chunks in the specified dimension.

### getCoordinatesForEntityClassInDim

- Syntax: `tpsCard.getCoordinatesForEntityClassInDim(className, dimension)`
- Parameters:
  - `className:string`: full Java entity class name.
  - `dimension:number`: dimension id.
- Returns:
  - Success: `table`
  - Permission failure: `nil, "Access denied"`
  - Dimension failure: `nil, "Dimension not loaded: <id>"`
- Purpose: Returns the coordinates of every loaded entity in the target dimension whose runtime class exactly matches `className`.

Return format:

- The returned table contains one entry per matching entity.
- Each entry is a three-value coordinate tuple in the form `{x, y, z}`.

## Example

```lua
local component = require("component")

local tpsCard = component.tps_card
local ms = tpsCard.getOverallTickTime()

print("Overall tick time (ms):", ms)
print("Overall TPS:", tpsCard.convertTickTimeIntoTps(ms))
print("Loaded dimensions:", tpsCard.getOverallDimsLoaded())
print("Loaded chunks:", tpsCard.getOverallChunksLoaded())

local dims = tpsCard.getAllDims()
for id, name in pairs(dims) do
  print("Dimension", id, name, tpsCard.getTickTimeInDim(id))
end
```

## Related

- `debug`
- `computer`
