# enderstorage

## Summary

This page documents the OpenComputers integration for EnderStorage frequency-owned containers.
The exposed callbacks let Lua code inspect the currently selected frequency, change it, and determine whether the channel belongs to a player or to the global namespace.

## Availability

- Dependency: `enderstorage`
- Label: `integration-required`

## Component Family

These callbacks are exposed by EnderStorage-backed frequency-owner environments, such as blocks or items that store and use an EnderStorage frequency.

## Common Behavior

- The frequency is returned as the packed numeric value used by the underlying EnderStorage implementation.
- In practice this value represents the three-color channel selection plus its public or owner-scoped namespace behavior.
- Owner-scoped channels usually report a player name.
- Public channels commonly report `global`.

## Interface Details

### `getFrequency`

- Syntax: `component.getFrequency()`
- Returns: number
- Purpose: reads the current EnderStorage frequency value

Use this when you want to snapshot the current channel before retuning a chest, tank, or another EnderStorage-compatible endpoint.

Example:

```lua
local frequency = storage.getFrequency()
print("Current frequency:", frequency)
```

### `setFrequency`

- Syntax: `component.setFrequency(value)`
- Parameters:
  - `value`: numeric frequency to apply
- Returns: boolean
- Purpose: switches the target to another EnderStorage channel

This changes the active frequency immediately when the underlying target accepts the new value.

Example:

```lua
local ok = storage.setFrequency(123)
print("Frequency changed:", ok)
```

### `getOwner`

- Syntax: `component.getOwner()`
- Returns: string
- Purpose: reads the owner namespace for the current channel

This is useful when you need to distinguish a private player-owned channel from a shared global one.

Example:

```lua
print("Owner:", storage.getOwner())
```

## Full Example

```lua
local component = require("component")
local storage = component.proxy(component.list()())

print("Owner:", storage.getOwner())
print("Current frequency:", storage.getFrequency())

local ok = storage.setFrequency(123)
print("Frequency changed:", ok)
```

## Related

- `modem`
- `inventory_controller`
