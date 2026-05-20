# uname

## Summary

Print the SecureOS version string, optionally together with the bundled kernel version.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\uname.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\uname`

## Syntax

```lua
uname
```
```lua
uname -a
```

## Purpose

Write `_OSVERSION` by default. With `-a`, append `_VERSION` so the output includes both the user-facing operating system version and the bundled kernel/runtime version.

## Options

- `-a`: Print both the operating system version and the bundled kernel/runtime version.

## Examples

```lua
uname
```
Print only the current SecureOS version string.

```lua
uname -a
```
Print the SecureOS version together with the bundled kernel/runtime version.
