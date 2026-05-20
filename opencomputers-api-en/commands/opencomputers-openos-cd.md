# cd

## Summary

Change the shell working directory.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\cd.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cd`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cd`

## Syntax

```lua
cd [DIRECTORY]
```
```lua
cd -
```

## Purpose

Update the current shell working directory used to resolve relative paths. Without an argument the command switches to `HOME`; with `-` it switches to `OLDPWD` and prints the resulting directory.

## Parameters

- `DIRECTORY`: Optional target directory. If omitted, use the path stored in `HOME`.

## Options

- `--help`: Print a short usage message.

## Examples

```lua
cd /bin
```
Change the current working directory to `/bin`.

```lua
cd ../
```
Move from the current directory to its parent directory.

```lua
cd -
```
Jump back to the previous working directory stored in `OLDPWD`.

## Notes

- A missing `HOME` or `OLDPWD` environment variable causes the command to fail before path resolution starts.
- On success the command updates `OLDPWD` to the directory that was active before the change.
