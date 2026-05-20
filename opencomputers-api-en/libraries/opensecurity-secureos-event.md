# event

## Summary

Signal dispatch, timers, and filtered event waiting for OpenOS programs.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\event.lua`

## Usage

```lua
local event = require("event")
```

## API

### event.cancel(timerId)

- Description: Cancel a timer or registered handler by identifier.
- Parameters:
  - `timerId`

```lua
event.cancel(nil)
```

### event.ignore(name, callback)

- Description: Remove a listener for a named signal.
- Parameters:
  - `name`
  - `callback`

```lua
event.ignore("value", nil)
```

### event.listen(name, callback)

- Description: Register a callback for a named signal if it is not already registered.
- Parameters:
  - `name`
  - `callback`

```lua
event.listen("value", nil)
```

### event.onError(message)

- Description: Handle asynchronous errors raised by event callbacks.
- Parameters:
  - `message`

```lua
event.onError(nil)
```

### event.pull(...)

- Description: Wait for the next signal, optionally filtering by name or timeout.
- Parameters:
  - `...`

```lua
event.pull(nil)
```

### event.pullFiltered(...)

- Description: Wait for the next signal accepted by a custom filter function.
- Parameters:
  - `...`

```lua
event.pullFiltered(nil)
```

### event.pullMultiple(...)

- Description: Wait for the next signal matching any of the provided names.
- Parameters:
  - `...`

```lua
event.pullMultiple(nil)
```

### event.shouldInterrupt()

- Description: Return whether the current input state should raise an interrupt.

```lua
event.shouldInterrupt()
```

### event.shouldSoftInterrupt()

- Description: Return whether the current input state should raise a soft interrupt.

```lua
event.shouldSoftInterrupt()
```

### event.timer(interval, callback, times)

- Description: Create a timer that invokes a callback later or repeatedly.
- Parameters:
  - `interval`
  - `callback`
  - `times`

```lua
event.timer(0, nil, nil)
```

## Notes

- Only exported module functions are listed on this page.
