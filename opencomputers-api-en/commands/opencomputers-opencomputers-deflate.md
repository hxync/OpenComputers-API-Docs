# deflate

## Summary

Compress data with the data-card deflate routine.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\data\usr\bin\deflate.lua`

## Syntax

```lua
deflate
```
```lua
deflate FILE
```

## Purpose

Read either standard input or the specified file completely into memory, then write the deflated result to standard output.

## Parameters

- `FILE`: Optional file to compress. If omitted, read from standard input.

## Examples

```lua
deflate document.txt > document.deflated
```
Compress `document.txt` and redirect the compressed output into `document.deflated`.

```lua
cat document.txt | deflate
```
Compress the piped input stream.

## Notes

- This implementation loads the full input into memory before compressing it.
