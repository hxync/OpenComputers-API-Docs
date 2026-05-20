# sh

## Summary

Start the standard interactive Plan9k shell.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sh.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`

## Syntax

```lua
sh
```

## Purpose

Plan9k ships an interactive shell with the same basic command language described by the bundled `sh` manual: quoted arguments, environment variable expansion, globbing, redirects, pipelines, and aliases. It is the default user-facing command interpreter for Plan9k terminal sessions.

## Examples

```lua
sh
```
Open a new interactive Plan9k shell session.

```lua
cp /bin/* /usr/bin
```
Use shell globbing to copy multiple files in one command.

```lua
cat < input.txt >> output.txt
```
Read from `input.txt` and append the result to `output.txt` using redirects.

## Notes

- The bundled documentation for this shell is shared with the OpenOS `sh` manual, so the described feature set is effectively the same.
