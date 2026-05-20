# sh

## Summary

OpenOS shell execution helpers for alias expansion, variable substitution, piping, and completion hooks.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\sh.lua`

## Usage

```lua
local sh = require("sh")
```

## API

### sh.execute(env, command, ...)

- Description: Parse and execute one shell command line, updating the shell's remembered last exit code.
- Parameters:
  - `env`: Environment table passed to child processes when appropriate.
  - `command`: Command line string to parse and execute.
  - `...`: Extra arguments passed to the final pipeline stage or direct child process.

```lua
sh.execute(_ENV, "echo $HOME")
```

### sh.expand(value)

- Description: Expand shell variables such as `$HOME`, `$?`, or `${NAME}` inside one already-tokenized text fragment.
- Parameters:
  - `value`: Single text fragment to expand.

```lua
local expanded = sh.expand("last=$?")
```

### sh.getLastExitCode()

- Description: Return the last shell-style numeric exit code recorded by `sh.execute`.

```lua
local ec = sh.getLastExitCode()
```

### sh.hintHandler(full_line, cursor)

- Description: Return completion candidates for one command line and cursor position.
- Parameters:
  - `full_line`: Full command line currently being edited.
  - `cursor`: 1-based cursor position inside `full_line`.

```lua
local hints = sh.hintHandler('ls /et', 7)
```

## Notes

- The small bootstrap module covers simple execution and expansion; more advanced parsing, globbing, and redirection helpers are loaded lazily from `full_sh`.
