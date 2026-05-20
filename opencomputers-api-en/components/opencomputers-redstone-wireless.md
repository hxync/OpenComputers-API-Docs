# opencomputers-redstone-wireless

## Summary

This page documents the wireless-redstone callbacks added to advanced `redstone` components when a supported wireless redstone integration is available.

## Availability

- Repository: `opencomputers`
- Actual Lua component name: `redstone`
- Backing trait: `RedstoneWireless`
- External dependency: a supported wireless redstone mod such as WR-CBE

## What It Does

- Tracks one wireless redstone frequency.
- Reads whether the tuned wireless channel is currently powered.
- Enables or disables this device's own wireless transmission state.
- Retunes the device to a different wireless frequency.

## Signal Behavior

Wireless input changes are converted into the same OpenComputers signal used by wired redstone:

```lua
"redstone_changed", "wireless", oldValue, newValue
```

For wireless changes, the side field is the literal string `"wireless"` instead of a numeric side.

## API

### getWirelessInput

- Syntax: `redstone.getWirelessInput()`
- Returns: `boolean`
- Purpose: Polls the current wireless input state on the configured frequency.

Even though some generated docs label this as a number, the implementation returns a boolean power state.

### getWirelessOutput

- Syntax: `redstone.getWirelessOutput()`
- Returns: `boolean`
- Purpose: Checks whether this device is currently transmitting a powered wireless signal.

### setWirelessOutput

- Syntax: `redstone.setWirelessOutput(value)`
- Parameters:
  - `value:boolean`: desired transmit state.
- Returns: `boolean`
- Purpose: Changes wireless transmit state and returns the previous transmit state.

### getWirelessFrequency

- Syntax: `redstone.getWirelessFrequency()`
- Returns: `number`
- Purpose: Reads the currently configured wireless frequency.

### setWirelessFrequency

- Syntax: `redstone.setWirelessFrequency(frequency)`
- Parameters:
  - `frequency:number`: new wireless frequency.
- Returns: `number`
- Purpose: Changes the wireless frequency and returns the previous frequency.

Important side effect:

- Retuning resets cached wireless input and forces wireless output back to `false`.

## Practical Notes

- Changing frequency pauses the calling context briefly.
- Disconnecting the component from the network removes its wireless transmitter and receiver registration and resets the stored frequency to `0`.
- Wireless wake-up still uses the common redstone wake threshold logic through `redstone_changed`.

## Example

```lua
local component = require("component")
local redstone = component.redstone

print("Old frequency:", redstone.setWirelessFrequency(1234))
print("Input powered:", redstone.getWirelessInput())

local oldOutput = redstone.setWirelessOutput(true)
print("Previous transmit state:", oldOutput)
print("Current transmit state:", redstone.getWirelessOutput())
```

## Related

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-signaller`
