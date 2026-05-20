# resolution

## Summary

Print the current screen resolution or switch to a new one.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\resolution.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\resolution`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\resolution`

## Syntax

```lua
resolution
```
```lua
resolution WIDTH HEIGHT
```

## Purpose

Without arguments, print the active width and height. With two numeric arguments, request a new resolution from the current GPU and clear the terminal after the change succeeds.

## Parameters

- `WIDTH`: Target screen width in characters.
- `HEIGHT`: Target screen height in characters.

## Examples

```lua
resolution
```
Print the current screen resolution.

```lua
resolution 80 25
```
Request an `80x25` text resolution and clear the screen if it succeeds.

## Notes

- The requested size must be supported by the currently bound GPU and screen.
- Invalid or unsupported sizes cause the command to print an error instead of changing the display.
