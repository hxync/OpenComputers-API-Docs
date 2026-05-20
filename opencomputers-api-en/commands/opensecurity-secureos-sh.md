# sh

## Summary

Start the standard interactive SecureOS shell.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\sh.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\sh`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\sh`

## Syntax

```lua
sh
```

## Purpose

This is SecureOS's built-in standard shell. It supports quoted arguments, environment variable expansion, basic globbing with `*` and `?`, basic redirects and pipelines, and alias handling through `alias`, `unalias`, or the `shell` API.

## Examples

```lua
sh
```
Open a new interactive SecureOS shell session.

```lua
echo 'a b'
```
Print a literal string with spaces and without variable expansion.

```lua
ls | cat > files.txt
```
Use a simple pipe and redirect chain in SecureOS shell.

## Notes

- Compared with the OpenOS manual text, the bundled SecureOS description explicitly labels its globbing, redirects, and piping support as basic.
