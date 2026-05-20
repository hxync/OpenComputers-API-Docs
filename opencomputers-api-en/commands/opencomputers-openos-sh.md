# sh

## Summary

Start the standard interactive OpenOS shell.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\sh.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`

## Syntax

```lua
sh
```

## Purpose

This is OpenOS's built-in command interpreter. It resolves the first token as a program name, passes the remaining tokens as arguments, expands environment variables such as `$NAME` and `${NAME}`, supports quoting with single or double quotes, performs glob expansion for `*` and `?`, and implements redirects, pipelines, and aliases.

## Examples

```lua
sh
```
Open a new interactive OpenOS shell session.

```lua
echo "a b"
```
Print a string containing spaces by quoting it.

```lua
ls | cat > files.txt
```
Pipe the output of `ls` through `cat` and redirect it into `files.txt`.

## Notes

- Single quotes suppress variable expansion, while double quotes still allow it.
- Aliases can be created with `alias` and removed with `unalias` or the `shell` API.
