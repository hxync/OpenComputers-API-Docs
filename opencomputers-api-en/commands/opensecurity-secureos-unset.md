# unset

## Summary

Remove one or more environment variables.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\unset.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unset`

## Syntax

```lua
unset [VARNAME]
```

## Purpose

For each provided variable name, the command clears it by calling `os.setenv(name, nil)`.

## Parameters

- `VARNAME`: Name of an environment variable to remove.

## Examples

```lua
unset some_variable
```
Remove the environment variable `some_variable`.
