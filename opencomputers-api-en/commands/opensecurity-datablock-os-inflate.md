# os_inflate

## Summary

Decompress data with the DataBlock inflate routine.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_inflate.lua`

## Syntax

```lua
os_inflate
```
```lua
os_inflate FILE
```

## Purpose

Read all data from standard input or the first file argument, then write the inflated result to standard output.

## Parameters

- `FILE`: Optional file to decompress. If omitted, read from standard input.

## Examples

```lua
os_inflate archive.deflated > archive.txt
```
Decompress `archive.deflated` and redirect the restored data.

## Notes

- The implementation loads the full input into memory before decompressing it.
