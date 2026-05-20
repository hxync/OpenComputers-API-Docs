# less

## Summary

View a file or piped text with a scrollable buffered pager.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\less.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\less`

## Syntax

```lua
less [FILE]
```

## Purpose

Behave like `more`, but keep a buffer so you can move backward and forward one line or one page at a time.

## Parameters

- `FILE`: Optional file to open. If omitted, read from standard input.

## Examples

```lua
less example.txt
```
Display the contents of `example.txt` in the pager.

```lua
find / | less
```
Pipe command output into `less` for interactive scrolling.

## Notes

- Key bindings from the bundled manual are `q`, arrow up/down, space, PageDown, PageUp, Home, and End.
