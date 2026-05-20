# eeprom

## Summary

The `eeprom` component controls an EEPROM chip's boot code, label, checksum, readonly state, and extra volatile data area. This is the component used to store BIOS-like startup code for computers, and it is also the main programmable storage medium for devices such as drones and microcontrollers.

The component exposes two distinct byte arrays:

- the main EEPROM code area,
- a separate volatile data area for auxiliary state.

These two areas have different sizes and different readonly behavior.

## Availability

- Repository: `opencomputers`
- Component name: `eeprom`
- Typical hosts: EEPROM item, computers, drones, microcontrollers, tablets, servers

## Important Notes

- `get` / `set` work on the main boot code area.
- `getData` / `setData` work on the extra volatile data area.
- `makeReadonly` permanently locks the main code and label against further modification.
- `setData` is still allowed after readonly in the source implementation.

## Usage

```lua
local component = require("component")
local eeprom = component.eeprom

if not eeprom then
  error("eeprom component not available")
end
```

## API

### get

- Syntax: `eeprom.get()`
- Returns: `string`
- Purpose: Reads the currently stored main EEPROM byte array.

This is the program or boot code area the firmware uses.

Example:

```lua
local code = eeprom.get()
print("Code bytes:", #code)
```

### set

- Syntax: `eeprom.set(data)`
- Returns:
  - Success: no direct return values
  - Failure: `nil, reason`
- Purpose: Replaces the main EEPROM byte array.

Parameter:

- `data:string` optional: raw byte content to store. Omitting it writes an empty byte array.

Behavior:

- Fails with `nil, "storage is readonly"` when the EEPROM has been locked.
- Fails with `nil, "not enough energy"` when the computer buffer is too low.
- Raises `"not enough space"` when the supplied data exceeds EEPROM code capacity.
- Pauses the computer for `2` seconds in source.

### getLabel

- Syntax: `eeprom.getLabel()`
- Returns: `string`
- Purpose: Reads the EEPROM label.

### setLabel

- Syntax: `eeprom.setLabel(data)`
- Returns:
  - Success: `string`
  - Failure: `nil, reason`
- Purpose: Changes the EEPROM label and returns the stored result.

Parameter:

- `data:string` optional: new label text.

Behavior:

- Fails with `nil, "storage is readonly"` after readonly has been enabled.
- The label is trimmed, truncated to 24 characters, and defaults back to `"EEPROM"` when empty.

### getSize

- Syntax: `eeprom.getSize()`
- Returns: `number`
- Purpose: Gets the capacity of the main EEPROM code area in bytes.

### getChecksum

- Syntax: `eeprom.getChecksum()`
- Returns: `string`
- Purpose: Gets the CRC32 checksum of the current main EEPROM code data.

This checksum is what `makeReadonly` expects as confirmation.

### makeReadonly

- Syntax: `eeprom.makeReadonly(checksum)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Permanently switches the EEPROM into readonly mode.

Parameter:

- `checksum:string`: current checksum returned by `getChecksum()`.

Behavior:

- Success is irreversible.
- If the checksum does not match current code data, the call returns `nil, "incorrect checksum"`.

Example:

```lua
local checksum = eeprom.getChecksum()
local ok, reason = eeprom.makeReadonly(checksum)
print(ok, reason)
```

### getDataSize

- Syntax: `eeprom.getDataSize()`
- Returns: `number`
- Purpose: Gets the capacity of the auxiliary volatile data area.

This size is separate from the main program size returned by `getSize()`.

### getData

- Syntax: `eeprom.getData()`
- Returns: `string`
- Purpose: Reads the auxiliary volatile byte array.

This area is commonly used for small extra state that should travel with the EEPROM.

### setData

- Syntax: `eeprom.setData(data)`
- Returns:
  - Success: no direct return values
  - Failure: `nil, reason`
- Purpose: Replaces the auxiliary volatile byte array.

Parameter:

- `data:string` optional: raw byte content to store.

Behavior:

- Unlike `set`, this is not blocked by the readonly flag in source.
- Fails with `nil, "not enough energy"` when power is insufficient.
- Raises `"not enough space"` when the data exceeds volatile data capacity.
- Pauses the computer for `1` second in source.

Example:

```lua
eeprom.setData("cfg=1")
print("Volatile bytes:", #eeprom.getData())
```

### Boot Code Example

```lua
local program = [[
return function()
  computer.beep(1000, 0.1)
end
]]

local ok, reason = eeprom.set(program)
if ok == nil then
  io.stderr:write("EEPROM write failed: " .. tostring(reason) .. "\n")
end
```

## Related

- `computer`
- `drive`
- `filesystem`
- `component`
