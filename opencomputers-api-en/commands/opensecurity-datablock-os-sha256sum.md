# os_sha256sum

## Summary

Compute SHA-256 digests with the DataBlock helper library.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_sha256sum.lua`

## Syntax

```lua
os_sha256sum [FILE...]
```

## Purpose

Read standard input or one or more files completely, compute SHA-256, and print hexadecimal digest text.

## Parameters

- `FILE...`: Optional files to hash. If omitted, read from standard input.

## Examples

```lua
os_sha256sum file.bin
```
Print the SHA-256 digest of `file.bin` and the file name.

## Notes

- The implementation loads each whole file into memory before hashing it.
