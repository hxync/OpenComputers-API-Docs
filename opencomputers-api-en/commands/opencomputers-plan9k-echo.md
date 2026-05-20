# echo

## Summary

Print text to standard output.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\echo.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\echo`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\echo`

## Syntax

```lua
echo [STRING...]
```

## Purpose

Write the supplied strings to standard output. By default, `echo` appends a trailing newline; with `-n`, it leaves the final newline off.

## Parameters

- `STRING...`: One or more text fragments to print in order.

## Options

- `-n`: Do not print the trailing newline.
- `--help`: Print the built-in help text and exit.

## Examples

```lua
echo test
```
Print `test` followed by a newline.

```lua
echo "a   b"
```
Print the text `a   b` exactly as supplied.

```lua
echo -n prompt>
```
Print `prompt>` without moving to the next line.
