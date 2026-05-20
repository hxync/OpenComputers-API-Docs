# keyboard

## Summary

Track pressed keys and test common modifier or control-key state.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\keyboard.lua`

## Usage

```lua
local keyboard = require("keyboard")
```

## API

### keyboard.isAltDown()

- Description: Return whether either Alt key is currently pressed.

```lua
keyboard.isAltDown()
```

### keyboard.isControl(char)

- Description: Return whether a numeric character code falls into the control-character ranges.
- Parameters:
  - `char`

```lua
keyboard.isControl(nil)
```

### keyboard.isControlDown()

- Description: Return whether either Control key is currently pressed.

```lua
keyboard.isControlDown()
```

### keyboard.isKeyDown(charOrCode)

- Description: Return whether a specific character or key code is currently marked as pressed.
- Parameters:
  - `charOrCode`

```lua
keyboard.isKeyDown(nil)
```

### keyboard.isShiftDown()

- Description: Return whether either Shift key is currently pressed.

```lua
keyboard.isShiftDown()
```

## Notes

- The module also exposes a `keys` table; during boot it contains only a minimal subset, and the full key map is delayed until requested.
