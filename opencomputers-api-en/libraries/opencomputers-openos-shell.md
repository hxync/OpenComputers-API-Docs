# shell

## Summary

Shell path resolution, alias management, and argument parsing helpers.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\shell.lua`

## Usage

```lua
local shell = require("shell")
```

## API

### shell.getAlias(alias)

- Description: Look up the value of a shell alias.
- Parameters:
  - `alias`

```lua
shell.getAlias(nil)
```

### shell.getShell()

- Description: Load and return the configured shell entry point.

```lua
shell.getShell()
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

### shell.prime()

- Description: Ensure per-process shell alias and variable tables exist.

```lua
shell.prime()
```

### shell.resolve(path, ext)

- Description: Resolve a path or command name against the current directory and `PATH`.
- Parameters:
  - `path`
  - `ext`

```lua
shell.resolve(nil, nil)
```

### shell.setAlias(alias, value)

- Description: Assign or clear a shell alias.
- Parameters:
  - `alias`
  - `value`

```lua
shell.setAlias(nil, nil)
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
