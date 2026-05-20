# source

## Summary

Execute a shell script file line by line in a child shell process.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\source.lua`

## Syntax

```lua
source FILE
```

## Purpose

Read one file line by line, start a fresh shell process for each line, and continue it with the current environment table. Aliases and shell variables are explicitly shared back into the child process so changes can propagate.

## Parameters

- `FILE`: Script file to execute line by line.

## Options

- `-q`: Quietly suppress the open-file error message when the file cannot be sourced.

## Examples

```lua
source /etc/profile
```
Execute each line from `/etc/profile` through the current shell program.

## Notes

- The command requires exactly one file argument.
- Execution is line-oriented, so it does not behave exactly like a traditional shell `source` that parses the whole file as one script body.
