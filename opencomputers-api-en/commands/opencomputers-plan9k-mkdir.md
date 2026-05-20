# mkdir

## Summary

Create one or more directories.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\mkdir.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mkdir`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mkdir`

## Syntax

```lua
mkdir DIRECTORY...
```

## Purpose

Create each specified directory path. The command resolves every argument through the current shell working directory before calling the filesystem library.

## Parameters

- `DIRECTORY...`: One or more directory paths to create.

## Examples

```lua
mkdir a
```
Create directory `a` in the current directory.

```lua
mkdir /a/b c
```
Create `/a`, then `/a/b`, and create `c` in the current directory if they do not already exist.

## Notes

- When creation fails, the command prints the resolved target path and the filesystem error.
