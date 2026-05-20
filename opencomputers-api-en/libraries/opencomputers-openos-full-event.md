# event

## Summary

Signal dispatch, timers, and filtered event waiting for OpenOS programs.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_event.lua`

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

### event.onError(message)

- Description: Handle asynchronous errors raised by event callbacks.
- Parameters:
  - `message`

```lua
event.onError(nil)
```

### event.pullMultiple(...)

- Description: Wait for the next signal matching any of the provided names.
- Parameters:
  - `...`

```lua
event.pullMultiple(nil)
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
