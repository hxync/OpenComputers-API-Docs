# df

## Summary

Report filesystem capacity and free-space information.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\df.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\df`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\df`

## Syntax

```lua
df [FILE]...
```

## Purpose

Show disk-space information for the filesystems that contain the specified paths. When no path is supplied, the command reports on every currently mounted filesystem.

## Parameters

- `FILE...`: Optional paths used to select which mounted filesystems should be reported.

## Examples

```lua
df
```
Show global disk-usage information for all mounted filesystems.

```lua
df /home /var
```
Show the capacity information for the filesystems mounted at `/home` and `/var`.
