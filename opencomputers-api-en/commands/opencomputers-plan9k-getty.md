# getty

## Summary

Render a Plan9k pseudo-terminal stream onto a GPU and screen pair.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\getty.lua`

## Syntax

```lua
getty GPU_ADDRESS SCREEN_ADDRESS
```

## Purpose

This is Plan9k's terminal display worker. It reads characters from standard input, interprets cursor movement, color, clearing, scrolling, and several Plan9k-specific escape sequences, and draws the result through the target GPU.

## Parameters

- `GPU_ADDRESS`: GPU component address used for drawing.
- `SCREEN_ADDRESS`: Screen component address that the GPU should already be bound to or will be used with.

## Examples

```lua
getty 1a2 3b4
```
Start a display renderer for the GPU whose address starts with `1a2` and the screen whose address starts with `3b4`.

## Notes

- This program is normally launched by Plan9k `init` and is not meant to be used as a regular interactive shell command.
- It supports standard ANSI-style escape handling plus Plan9k `ESC 9` extensions for resolution changes, copying regions, and terminal metadata exchange.
