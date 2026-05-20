# lua

## Summary

Launch the Lua interpreter, or execute a Lua script entry file.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\lua.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\de_DE\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\en_US\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\fr_FR\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\ru_RU\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\zh_CN\general\lua.md`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\lua`

## Syntax

```lua
lua
```
```lua
lua FILE [ARGS...]
```

## Purpose

When called without arguments, the command starts the bundled Lua shell entry script. When a filename is provided, it reads that file, strips a shebang if present, loads it, and executes it with the remaining arguments.

## Parameters

- `FILE`: Optional Lua script to execute instead of the interactive shell.
- `ARGS...`: Optional extra arguments forwarded to the loaded script.

## Examples

```lua
lua
```
Launch the interactive Lua interpreter shell.

```lua
lua /home/test.lua foo bar
```
Execute `/home/test.lua` and pass `foo` and `bar` as script arguments.

## Notes

- The interpreter shell will attempt to resolve unknown globals by calling `require` with that name.
- Syntax or runtime errors are written to standard error and cause a false exit status.
