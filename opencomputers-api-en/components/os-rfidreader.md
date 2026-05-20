# os_rfidreader

## Summary

The `os_rfidreader` component scans nearby RFID-bearing targets and reports them through both return tables and `rfidData` signals. OpenSecurity exposes this component in two forms:

- a placed RFID reader block
- an RFID reader card upgrade

Both surfaces use the same component name and nearly the same callback behavior.

## Availability

- Repository: `opensecurity`
- Component name: `os_rfidreader`
- Typical hosts:
  - placed OpenSecurity RFID reader block
  - OpenSecurity RFID reader card inside supported devices such as tablets

## Usage

```lua
local component = require("component")
local reader = component.os_rfidreader

if not reader then
  error("os_rfidreader component not installed")
end
```

## What Gets Detected

The scan logic looks for RFID data in three places:

- RFID cards in player inventories
- RFID cards in OpenComputers drone inventories
- entity NBT data under `entityData.rfidData`

Each found RFID payload sends:

```lua
computer.signal("rfidData", name, range, data, uuid)
```

Signal fields:

- `name`: player display name, drone/entity name, or other entity command sender name
- `range`: distance from the host device
- `data`: RFID payload string
- `uuid`:
  - real UUID normally
  - `"-1"` when `ignoreUUIDs` is enabled

The returned scan table also includes:

- `locked:boolean`

## Range And Energy

- Requested range is clamped to `OpenSecurity.rfidRange`.
- The configured default max is `16`, and the config allows `1` through `64`.
- After clamping, the implementation divides the range by `2`.
- Energy cost is `5 * effectiveRadius`.

Block versus card difference:

- The placed block returns `false, "Not enough power in OC Network."` on low power.
- The card driver throws an exception with the same message.

## API

### greet

- Syntax: `reader.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

### scan

- Syntax: `reader.scan([range])`
- Returns:
  - Success: `table`
  - Failure:
    - block reader: `false, reason`
    - card reader: exception
- Purpose: Scans the surrounding area for RFID payloads.

Parameters:

- `range:number` optional: requested radius before clamping and halving.

Returned table entry format:

- `entry.name`
- `entry.range`
- `entry.data`
- `entry.uuid`
- `entry.locked`

Behavior:

- One returned table entry is created for each RFID payload found.
- A single player or drone can contribute multiple entries if carrying multiple RFID cards.
- An empty table is returned when nothing is found.

Example:

```lua
local results, reason = reader.scan(16)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.data, entry.uuid, entry.locked)
end
```

## Example

```lua
local event = require("event")

local results = reader.scan(16)
if results == false then
  error("scan failed")
end

for _, entry in pairs(results) do
  print("Found RFID:", entry.name, entry.data)
end

while true do
  local _, _, _, eventName, name, range, data, uuid = event.pull("computer.signal")
  if eventName == "rfidData" then
    print(name, range, data, uuid)
  end
end
```

## Related

- `component`
- `os_magreader`
- `os_cardwriter`
