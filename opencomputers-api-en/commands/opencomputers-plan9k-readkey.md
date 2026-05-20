# readkey

## Summary

Translate keyboard and clipboard events into terminal input bytes for a screen session.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\readkey.lua`

## Syntax

```lua
readkey SCREEN_ADDRESS
```

## Purpose

Watch all keyboards currently attached to the specified screen, forward normal characters to standard output, and encode special keys such as arrows, function keys, delete, home, and page navigation as escape sequences.

## Parameters

- `SCREEN_ADDRESS`: Screen component whose attached keyboards should be observed.

## Examples

```lua
readkey 3b4
```
Convert keyboard events from the screen whose address starts with `3b4` into stdin-style bytes.

## Notes

- This helper is usually paired with `getty` and a shell process inside a Plan9k pseudo-terminal.
- It only tracks keyboards that were attached to the screen when the process started.
