# os_base64

## Summary

Encode or decode Base64 data with the DataBlock helper library.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_base64.lua`

## Syntax

```lua
os_base64 [FILE...]
```
```lua
os_base64 -d [FILE...]
```
```lua
os_base64 --decode [FILE...]
```

## Purpose

Use `datablock.encode64` or `datablock.decode64` on standard input or the supplied files. Encoding reads three-byte chunks, while decoding reads four-character Base64 chunks.

## Parameters

- `FILE...`: Optional files to encode or decode. If omitted, read from standard input.

## Options

- `-d, --decode`: Decode Base64 input instead of encoding it.
- `-h, --help`: Print a short hint pointing to the `base64` manual entry.

## Examples

```lua
os_base64 file.bin
```
Encode `file.bin` and write the Base64 result to standard output.

```lua
os_base64 -d payload.txt
```
Decode the Base64 text in `payload.txt`.
