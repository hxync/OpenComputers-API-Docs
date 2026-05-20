# shell

## Summary

Shell path resolution, alias management, and argument parsing helpers.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_shell.lua`

## Usage

```lua
local shell = require("shell")
```

## API

### shell.aliases()

- Description: Iterate the alias table for the current process.

```lua
shell.aliases()
```

### shell.execute(command, env, ...)

- Description: Run a command string through the configured shell entry point and return the child result.
- Parameters:
  - `command`
  - `env`
  - `...`

```lua
shell.execute(nil, nil, nil)
```

### shell.getPath()

- Description: Return the current `PATH` environment variable.

```lua
shell.getPath()
```

### shell.setPath(value)

- Description: Replace the current `PATH` environment variable.
- Parameters:
  - `value`

```lua
shell.setPath(nil)
```

## Notes

- OpenOS exposes a small bootstrap shell API first, then delays additional helpers such as command execution and `PATH` access until the full module is loaded.
