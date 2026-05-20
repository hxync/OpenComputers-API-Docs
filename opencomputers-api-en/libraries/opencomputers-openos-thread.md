# thread

## Summary

Thread-like wrappers around coroutine-backed OpenOS processes with event-aware suspension and joining.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\thread.lua`

## Usage

```lua
local thread = require("thread")
```

## API

### thread.create(fp, ...)

- Description: Start a new managed thread around a Lua function and return the thread handle immediately.
- Parameters:
  - `fp`: Function that becomes the thread body.
  - `...`: Arguments passed to the thread function on its first resume.

```lua
local worker = thread.create(function(name) print(name) end, 'job-1')
```

### thread.current()

- Description: Return the thread handle associated with the currently running process stack, if any.

```lua
local self = thread.current()
```

### thread.waitForAll(threads, timeout)

- Description: Block until every thread in the list has stopped running or the timeout expires.
- Parameters:
  - `threads`: Array of thread handles returned by `thread.create`.
  - `timeout`: Optional maximum number of seconds to wait.

```lua
thread.waitForAll(workers, 5)
```

### thread.waitForAny(threads, timeout)

- Description: Block until any thread in the list stops running or the timeout expires.
- Parameters:
  - `threads`: Array of thread handles returned by `thread.create`.
  - `timeout`: Optional maximum number of seconds to wait.

```lua
thread.waitForAny(workers, 1)
```

## Notes

- Only exported module functions are listed on this page.
