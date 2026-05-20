# hostname

## Summary

Display, set, or refresh the machine hostname.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\hostname.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\hostname`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\hostname`

## Syntax

```lua
hostname [NEW NAME] [--update]
```

## Purpose

Read the hostname from `/etc/hostname`, update that file with a new value, or refresh the `HOSTNAME` environment variables from disk when `--update` is used.

## Parameters

- `NEW NAME`: Optional hostname value to write into `/etc/hostname`.

## Options

- `--update`: Refresh the `HOSTNAME` environment variables from `/etc/hostname` without printing to stdout.

## Examples

```lua
hostname
```
Print the currently configured hostname.

```lua
hostname test
```
Set the machine hostname to `test`.

```lua
hostname --update
```
Reload hostname-related environment variables from `/etc/hostname`.

## Notes

- If no hostname is configured and no new one is supplied, the command reports an error.
- Setting a new hostname writes the file first, then updates environment variables only when `--update` is active.
