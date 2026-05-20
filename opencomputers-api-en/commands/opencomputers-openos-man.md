# man

## Summary

Open a bundled manual page with the current pager.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\man.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\man`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\man`

## Syntax

```lua
man TOPIC
```

## Purpose

Search every directory listed in `MANPATH` for the requested topic, resolve the entry with `shell.resolve`, and open the first matching manual file through the pager named by `PAGER`.

## Parameters

- `TOPIC`: Program, library, or topic name to search in the manual path.

## Examples

```lua
man ls
```
Open the manual page for the `ls` command.

```lua
man shell
```
Display documentation for the `shell` library if it exists in `MANPATH`.

## Notes

- The command requires both `MANPATH` and `PAGER` to be configured sensibly in the shell environment.
- If no matching entry is found, the program prints `No manual entry for <topic>` and exits with an error.
