# base64

## Summary

Encode or decode Base64 data from standard input or files.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\base64.lua`

## Syntax

```lua
base64 [FILE...]
```
```lua
base64 -d [FILE...]
```
```lua
base64 --decode [FILE...]
```

## Purpose

Use the data-card-backed `data.encode64` or `data.decode64` functions to transform input data. Without file arguments the command reads from standard input.

## Parameters

- `FILE...`: Optional files to encode or decode. If omitted, read from standard input.

## Options

- `-d, --decode`: Decode Base64 data instead of encoding it.
- `-h, --help`: Print a short hint that points to the manual entry.

## Examples

```lua
base64 file.bin
```
Encode `file.bin` as Base64 and write the result to standard output.

```lua
base64 -d payload.txt
```
Decode the Base64 content of `payload.txt` and write the raw bytes to standard output.

## Notes

- Encoding reads three-byte chunks, while decoding reads four-character chunks.
