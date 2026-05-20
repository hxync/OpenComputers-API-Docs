# clear

## Summary

Clear the terminal and reset the cursor to the top-left corner.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\clear.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\clear`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\clear`

## Syntax

```lua
clear
```

## Purpose

Erase all text from the current screen using the active background and foreground colors, then place the cursor back at position `(1, 1)`.

## Examples

```lua
clear
```
Clear the terminal before drawing a fresh screen of output.
