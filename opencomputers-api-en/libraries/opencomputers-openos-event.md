# event

## Summary

Signal dispatch, timers, and filtered event waiting for OpenOS programs.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\event.lua`

## Usage

```lua
local event = require("event")
```

## API

### event.listen(name, callback)

- Description: Register a callback for a named signal if it is not already registered.
- Parameters:
  - `name`
  - `callback`

```lua
event.listen("value", nil)
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

### event.register(key, callback, interval, times, opt_handlers)

- Description: Register a low-level handler with an optional interval and repeat count.
- Parameters:
  - `key`
  - `callback`
  - `interval`
  - `times`
  - `opt_handlers`

```lua
event.register(nil, nil, 0, nil, nil)
```

## Notes

- Only exported module functions are listed on this page.
