# os_deflate

## Summary

Compress data with the DataBlock deflate routine.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_deflate.lua`

## Syntax

```lua
os_deflate
```
```lua
os_deflate FILE
```

## Purpose

Read all data from standard input or the first file argument, then write the deflated result to standard output.

## Parameters

- `FILE`: Optional file to compress. If omitted, read from standard input.

## Examples

```lua
os_deflate archive.txt > archive.deflated
```
Compress `archive.txt` and redirect the compressed result.

## Notes

- The implementation loads the full input into memory before compressing it.
