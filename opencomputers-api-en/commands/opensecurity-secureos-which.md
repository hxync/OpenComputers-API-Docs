# which

## Summary

Locate the resolved path or alias target of a command name.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\which.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\which`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\which`

## Syntax

```lua
which COMMAND
```

## Purpose

Resolve each given command name through the shell search path. If a name is not found as a file but exists as an alias, the alias target is reported instead.

## Parameters

- `COMMAND`: Command name to resolve.

## Examples

```lua
which ls
```
Print the full path to `ls`.

```lua
which doesntexist dir
```
Show a missing-command error for `doesntexist` and the alias expansion for `dir`.

## Notes

- When a lookup fails, the command writes an error message to standard error and exits with a failure status.
