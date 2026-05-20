# process

## Summary

Manage coroutine-backed processes, inherited environment state, and owned handles.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\process.lua`

## Usage

```lua
local process = require("process")
```

## API

### process.addHandle(handle, proc)

- Description: Register a handle under the current or specified process so it can be closed automatically on exit.
- Parameters:
  - `handle`
  - `proc`

```lua
process.addHandle(nil, nil)
```

### process.findProcess(co)

- Description: Resolve a coroutine back to its owning process record.
- Parameters:
  - `co`

```lua
process.findProcess(nil)
```

### process.info(levelOrThread)

- Description: Return process metadata such as path, environment, command name, and per-process data tables.
- Parameters:
  - `levelOrThread`

```lua
process.info(nil)
```

### process.load(path, env, init, name)

- Description: Create a new coroutine-backed process from a script path or function.
- Parameters:
  - `path`
  - `env`
  - `init`
  - `name`

```lua
process.load(nil, nil, nil, "value")
```

### process.removeHandle(handle, proc)

- Description: Remove a handle from a process handle list.
- Parameters:
  - `handle`
  - `proc`

```lua
process.removeHandle(nil, nil)
```

### process.running(level)

- Description: Compatibility helper that returns the current process path, environment, and command name.
- Parameters:
  - `level`

```lua
process.running(nil)
```

## Notes

- Only exported module functions are listed on this page.
