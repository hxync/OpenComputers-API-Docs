# inflate

## Summary

Decompress data with the data-card inflate routine.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\bin\inflate.lua`

## Syntax

```lua
inflate
```
```lua
inflate FILE
```

## Purpose

Read either standard input or the specified file completely into memory, then write the inflated result to standard output.

## Parameters

- `FILE`: Optional file to decompress. If omitted, read from standard input.

## Examples

```lua
inflate document.deflated > document.txt
```
Decompress `document.deflated` and redirect the plain output into `document.txt`.

```lua
cat document.deflated | inflate
```
Decompress the piped input stream.

## Notes

- This implementation loads the full input into memory before decompressing it.
