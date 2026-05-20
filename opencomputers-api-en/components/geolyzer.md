# geolyzer

## Summary

The `geolyzer` component analyzes nearby terrain and adjacent blocks. It is commonly used for ore prospecting, terrain sampling, automation that reacts to block properties, and GregTech underground fluid detection.

Unlike a simple block lookup, most geolyzer operations consume energy and may fail when the internal buffer is too low. Large area scans are also restricted by range and total volume.

## Availability

- Repository: `opencomputers`
- Component name: `geolyzer`
- Typical hosts: computer, robot, tablet, drone, microcontroller with the appropriate hardware

## Usage

```lua
local component = require("component")
local geolyzer = component.geolyzer

if not geolyzer then
  error("geolyzer not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### analyze

- Syntax: `geolyzer.analyze(side[, options])`
- Returns:
  - Success: `table`
  - Failure: `nil, reason`
- Purpose: Reads extended information about the block directly adjacent to the host on the specified side.

Parameters:

- `side:number`: side to inspect.
- `options:table` optional: extra scan options passed to the event system. Most scripts leave this empty.

Notes:

- This call only works when item stack inspection is enabled in the OpenComputers configuration.
- It consumes geolyzer scan energy.
- It may fail with:
  - `"not enabled in config"`
  - `"not enough energy"`
  - `"scan was canceled"`

Example:

```lua
local sides = require("sides")
local data, reason = geolyzer.analyze(sides.front)
if not data then
  io.stderr:write("Analyze failed: " .. tostring(reason) .. "\n")
else
  print("Block name:", data.name or "unknown")
  print("Hardness:", data.hardness or "n/a")
end
```

### canSeeSky

- Syntax: `geolyzer.canSeeSky()`
- Returns: `boolean`
- Purpose: Checks whether the block space directly above the host has a clear line of sight to the sky.

This is useful for outdoor detectors, solar checks, and weather-aware automation.

Example:

```lua
if geolyzer.canSeeSky() then
  print("Open sky above the device")
else
  print("Covered from above")
end
```

### isSunVisible

- Syntax: `geolyzer.isSunVisible()`
- Returns: `boolean`
- Purpose: Reports whether the sun is currently visible above the host.

This is stricter than `canSeeSky()`. It also depends on time of day and weather. Desert biomes can still report sunlight during conditions that would block it elsewhere.

Example:

```lua
if geolyzer.isSunVisible() then
  print("Direct sunlight is available")
else
  print("No direct sunlight right now")
end
```

### scan

- Syntax: `geolyzer.scan(x, z[, y, w, d, h][, ignoreReplaceable|options])`
- Returns:
  - Success: `table`
  - Failure: `nil, reason`
- Purpose: Samples block density values in a vertical column or a bounded 3D area relative to the host.

Parameters:

- `x:number`: relative X offset.
- `z:number`: relative Z offset.
- `y:number` optional: relative starting Y offset for 3D scans.
- `w:number` optional: width along X.
- `d:number` optional: depth along Z.
- `h:number` optional: height along Y.
- `ignoreReplaceable:boolean` optional: legacy shorthand. When `true`, replaceable blocks are excluded from the result.
- `options:table` optional: advanced scan options.

Behavior:

- If only `x` and `z` are supplied, the geolyzer scans one vertical column from `y = -32` through `y = 31`.
- When `y, w, d, h` are supplied, the total scanned volume must not exceed `64` blocks.
- Every coordinate in the requested area must stay within the configured geolyzer range.
- The call consumes energy and may fail with:
  - `"not enough energy"`
  - `"scan was canceled"`
- Invalid requests raise errors such as:
  - `"volume too large (maximum is 64)"`
  - `"location out of bounds"`

Example: scan a single vertical column under and above the device.

```lua
local densities, reason = geolyzer.scan(0, 0)
if not densities then
  io.stderr:write("Scan failed: " .. tostring(reason) .. "\n")
else
  for y, hardness in ipairs(densities) do
    print(y - 33, hardness)
  end
end
```

Example: scan a `4 x 4 x 4` cube starting eight blocks below the host.

```lua
local data = assert(({geolyzer.scan(-1, -1, -8, 4, 4, 4)})[1])
print("Scanned blocks:", #data)
```

### scanUndergroundFluids

- Syntax: `geolyzer.scanUndergroundFluids()`
- Returns: `table`
- Purpose: Reads GregTech underground fluid data for the current chunk.

Return fields usually include:

- `type:string`: localized fluid name.
- `quantity:number`: amount reported for the chunk.

This function is only useful when the installed modpack includes the GregTech underground fluid system that OpenComputers is integrating with.

Example:

```lua
local fluid = geolyzer.scanUndergroundFluids()
print("Fluid type:", fluid.type)
print("Quantity:", fluid.quantity)
```

### store

- Syntax: `geolyzer.store(side, dbAddress, dbSlot)`
- Returns:
  - Success: `boolean`
  - Failure: `nil, reason`
- Purpose: Writes an item representation of the adjacent block into a database component slot.

Parameters:

- `side:number`: side of the block to capture.
- `dbAddress:string`: component address of the target database.
- `dbSlot:number`: destination slot in that database.

Return value details:

- On success, the boolean tells you whether the destination slot previously contained a stack.
- On failure, common reasons are:
  - `"not enough energy"`
  - `"block has no registered item representation"`

Example:

```lua
local database = component.database
local sides = require("sides")

local replaced, reason = geolyzer.store(sides.down, database.address, 1)
if replaced == nil then
  io.stderr:write("Store failed: " .. tostring(reason) .. "\n")
else
  print("Previous stack replaced:", replaced)
end
```

## Related

- `component`
- `database`
- `robot`
- `tablet`
