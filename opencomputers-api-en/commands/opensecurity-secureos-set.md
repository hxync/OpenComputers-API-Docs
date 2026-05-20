# set

## Summary

Inspect environment variables or assign shell environment values.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\set.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\set`

## Syntax

```lua
set [VARIABLE]=[VALUE]
```

## Purpose

Without arguments, the command prints the current environment as `key='value'` pairs. Arguments in the form `NAME=value` update environment variables. Arguments without `=` are stored as positional parameters.

## Parameters

- `NAME=value`: Assign an environment variable.
- `ARG`: Store a positional argument value for `$1`, `$2`, and so on.

## Examples

```lua
set
```
Show all current environment variables.

```lua
set example="Hello World"
```
Set `example` to `Hello World`.

## Notes

- When the first positional argument without `=` is seen, the script clears existing positional parameters before repopulating them.
