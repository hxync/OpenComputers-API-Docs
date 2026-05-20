# rc

## Summary

Service registry and runlevel helper functions for `rc`-managed startup scripts.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\rc.lua`

## Usage

```lua
local rc = require("rc")
```

## API

### rc.allRunCommand(cmd, ...)

- Description: Run the same service command against every loaded `rc` script and collect the per-service results.
- Parameters:
  - `cmd`
  - `...`

```lua
rc.allRunCommand(nil, nil)
```

### rc.load(name, args)

- Description: Load a service definition so later `rc` commands can address it by name.
- Parameters:
  - `name`
  - `args`

```lua
rc.load("value", nil)
```

### rc.runCommand(name, cmd, ...)

- Description: Invoke a named command such as `start` or `stop` on a specific loaded service.
- Parameters:
  - `name`
  - `cmd`
  - `...`

```lua
rc.runCommand("value", nil, nil)
```

### rc.unload(name)

- Description: Remove a previously loaded service from the in-memory registry.
- Parameters:
  - `name`

```lua
rc.unload("value")
```

## Notes

- The small bootstrap module only seeds `rc.loaded`; the remaining helpers are added by the delayed full implementation used by normal OpenOS-style systems.
