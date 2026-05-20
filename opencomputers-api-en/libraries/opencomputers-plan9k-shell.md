# shell

## Summary

Shell path resolution, alias management, and argument parsing helpers.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\shell.lua`

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

### shell.getAlias(alias)

- Description: Look up the value of a shell alias.
- Parameters:
  - `alias`

```lua
shell.getAlias(nil)
```

### shell.getPath()

- Description: Return the current `PATH` environment variable.

```lua
shell.getPath()
```

### shell.getWorkingDirectory()

- Description: Return the current working directory, defaulting to `/`.

```lua
shell.getWorkingDirectory()
```

### shell.parse(...)

- Description: Split a command line into positional arguments and short or long option tables.
- Parameters:
  - `...`

```lua
shell.parse(nil)
```

### shell.resolve(path, ext)

- Description: Resolve a path or command name against the current directory and `PATH`.
- Parameters:
  - `path`
  - `ext`

```lua
shell.resolve(nil, nil)
```

### shell.resolveAlias(command, args)

- Description: Expand the first command token through the alias table before execution continues.
- Parameters:
  - `command`
  - `args`

```lua
shell.resolveAlias(nil, nil)
```

### shell.setAlias(alias, value)

- Description: Assign or clear a shell alias.
- Parameters:
  - `alias`
  - `value`

```lua
shell.setAlias(nil, nil)
```

### shell.setPath(value)

- Description: Replace the current `PATH` environment variable.
- Parameters:
  - `value`

```lua
shell.setPath(nil)
```

### shell.setWorkingDirectory(dir)

- Description: Change the current working directory after validating it exists.
- Parameters:
  - `dir`

```lua
shell.setWorkingDirectory(nil)
```

## Notes

- OpenOS exposes a small bootstrap shell API first, then delays additional helpers such as command execution and `PATH` access until the full module is loaded.
