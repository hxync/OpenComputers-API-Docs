# more

## Summary

Display a text file one screen at a time.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\more.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\more`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\more`

## Syntax

```lua
more FILE
```

## Purpose

Reads the target file line by line, wraps output to the current GPU width, and pauses after each screenful. Use Space or PageDown for the next page, Enter or Down Arrow for one more line, and q to quit.

## Parameters

- `FILE`: Path to the file to display.

## Examples

```lua
more /home/a.txt
```
Display `/home/a.txt` one screen at a time.

## Notes

- This implementation does not support scrolling backward.
- Long lines are wrapped with `text.wrap`, so they may advance the page unexpectedly on narrow screens.
