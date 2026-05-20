# tis3d

## Summary

This page documents the OpenComputers bridge for the TIS-3D serial adapter interface.
When available, it exposes a `serial_port` component that lets Lua exchange signed 16-bit values with a TIS-3D serial network.

## Availability

- Dependency: `tis3d`
- Label: `integration-required`

## Component Name

- `serial_port`

## Queue Model

The bridge keeps two independent FIFO queues:

- read queue: values written from the TIS-3D side and waiting for Lua to consume them
- write queue: values written by Lua and waiting for TIS-3D to consume them

Both queues have a capacity of `128` values.

## `serial_port`

`serial_port` is exposed by the TIS-3D serial adapter bridge environment.

### `setReading`

- Syntax: `serial_port.setReading(enabled)`
- Parameters:
  - `enabled`: boolean
- Returns: nothing
- Purpose: enables or disables inbound reception from the TIS-3D side

When reading is disabled, the TIS-3D side cannot enqueue new inbound values into the OpenComputers read queue.

Example:

```lua
component.serial_port.setReading(true)
```

### `read`

- Syntax: `serial_port.read()`
- Returns: one number, or no value when the queue is empty
- Purpose: removes and returns the oldest queued inbound value

Important behavior:

- values are exposed to Lua as ordinary signed numbers converted from Java `short`
- an empty queue is not an error and does not return an error string

Example:

```lua
local value = component.serial_port.read()
if value ~= nil then
  print("Received:", value)
end
```

### `write`

- Syntax: `serial_port.write(value)`
- Parameters:
  - `value`: 16-bit number to queue for TIS-3D
- Returns: `true` on success, or `false, "buffer full"` when the outgoing queue is full
- Purpose: appends one outbound serial value

If the write queue already contains `128` pending values, the new value is rejected.

Example:

```lua
local ok, reason = component.serial_port.write(42)
print("Write:", ok, reason)
```

## Practical Notes

- Lua `write()` feeds the queue that TIS-3D reads from.
- Lua `read()` consumes the queue that TIS-3D writes to.
- The bridge connects itself to the adapter network when needed.
- Buffered values are serialized with the adapter state, so queued data can survive chunk saves.

## Full Example

```lua
local component = require("component")
local port = component.serial_port

port.setReading(true)

local ok, reason = port.write(42)
print("Write:", ok, reason)

local value = port.read()
if value ~= nil then
  print("Received:", value)
end
```

## Related

- `modem`
- `redstone`
