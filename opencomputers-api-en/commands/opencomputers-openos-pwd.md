# pwd

## Summary

Print the current working directory, optionally resolving it to a physical path.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\pwd.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\pwd`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\pwd`

## Syntax

```lua
pwd
```
```lua
pwd -P
```

## Purpose

Write the shell's current working directory to standard output. With `-P`, resolve the path through `filesystem.realPath` first so symbolic links and mount aliases collapse to their physical target.

## Options

- `-P`: Print the fully resolved physical path instead of the logical shell path.

## Examples

```lua
pwd
```
Print the shell's current logical working directory.

```lua
pwd -P
```
Resolve the current directory to its real on-filesystem target before printing it.

## Notes

- If `filesystem.realPath` cannot resolve the current directory, the command prints an error and exits non-zero.
