# os_entdetector

## Summary

The `os_entdetector` component scans entities around an OpenSecurity entity detector. It can return the detector's own block position, scan only players, or scan every entity in range and send one `entityDetect` signal per match.

## Availability

- Repository: `opensecurity`
- Component name: `os_entdetector`
- Typical host: placed OpenSecurity entity detector

## Usage

```lua
local component = require("component")
local detector = component.os_entdetector

if not detector then
  error("os_entdetector component not installed")
end
```

## Range And Energy

- The configured maximum scan range is `OpenSecurity.rfidRange`.
- The config default is `16`, with an allowed config range of `1` through `64`.
- The callback clamps any requested range to that maximum.
- After clamping, the implementation divides the value by `2` and uses that halved number as the actual scan radius.
- Energy cost is `5 * effectiveRadius`.

Example:

- requested `16`
- effective radius `8`
- energy cost `40`

## Signals

Each detected target sends:

```lua
computer.signal("entityDetect", name, range, x, y, z)
```

Payload behavior:

- `name`: player display name or entity command sender name
- `range`: distance from the detector block
- `x`, `y`, `z`:
  - absolute world coordinates when `offset` is `false`
  - relative offsets from the detector when `offset` is `true`

## API

### greet

- Syntax: `detector.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

### getLoc

- Syntax: `detector.getLoc()`
- Returns: `x, y, z`
- Purpose: Returns the detector block coordinates.

Example:

```lua
local x, y, z = detector.getLoc()
print(x, y, z)
```

### scanPlayers

- Syntax: `detector.scanPlayers([range[, offset]])`
- Returns:
  - Success: `table`
  - Failure: `false, reason`
- Purpose: Scans for player entities in range.

Parameters:

- `range:number` optional: requested scan radius before the internal halving step.
- `offset:boolean` optional: whether returned coordinates should be relative to the detector.

Returned table entry format:

- `entry.name`
- `entry.range`
- `entry.x`
- `entry.y`
- `entry.z`

Behavior:

- Only multiplayer player entities are included.
- An empty table is returned when nothing is found.

Example:

```lua
local results, reason = detector.scanPlayers(16, true)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.x, entry.y, entry.z)
end
```

### scanEntities

- Syntax: `detector.scanEntities([range[, offset]])`
- Returns:
  - Success: `table`
  - Failure: `false, reason`
- Purpose: Scans all entities in range and reports them.

Parameters:

- `range:number` optional: requested scan radius before the internal halving step.
- `offset:boolean` optional: whether returned coordinates should be relative to the detector.

Behavior:

- Despite the old callback text, the implementation does not exclude players here.
- Players and non-player entities are both returned.
- An empty table is returned when nothing is found.

Example:

```lua
local results, reason = detector.scanEntities(12, false)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.range)
end
```

## Example

```lua
local event = require("event")

local results = assert(detector.scanPlayers(16, true))
for _, entry in pairs(results) do
  print("Player:", entry.name, entry.x, entry.y, entry.z)
end

while true do
  local _, _, _, name, range, x, y, z = event.pull("computer.signal")
  if name == "entityDetect" then
    print("Detected:", range, x, y, z)
  end
end
```

## Notes

- The detector flashes block metadata states while scanning, but that is only visual and does not affect callback usage.
- If the OC network lacks power, both scan callbacks return `false, "Not enough power in OC Network."`.

## Related

- `component`
- `os_rfidreader`
