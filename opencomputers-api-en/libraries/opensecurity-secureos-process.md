# process

## Summary

SecureOS coroutine-process helpers for loading scripts, inheriting environments, and querying process metadata.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\process.lua`

## Usage

```lua
local process = require("process")
```

## API

### process.info(levelOrThread)

- Description: Return process metadata for the current process, an ancestor level, or an explicit coroutine thread.
- Parameters:
  - `levelOrThread`

```lua
process.info(nil)
```

### process.install(path, name)

- Description: Install SecureOS process bookkeeping into the current coroutine, replacing `coroutine.create` and `load` with process-aware wrappers.
- Parameters:
  - `path`
  - `name`

```lua
process.install(nil, "value")
```

### process.load(path, env, init, name)

- Description: Load one script path as a new process coroutine, inheriting or overriding the environment as needed.
- Parameters:
  - `path`
  - `env`
  - `init`
  - `name`

```lua
process.load(nil, nil, nil, "value")
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
