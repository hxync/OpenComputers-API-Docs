# os_md5sum

## Summary

Compute MD5 digests with the DataBlock helper library.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_md5sum.lua`

## Syntax

```lua
os_md5sum [FILE...]
```

## Purpose

Read standard input or one or more files completely, compute MD5, and print hexadecimal digest text.

## Parameters

- `FILE...`: Optional files to hash. If omitted, read from standard input.

## Examples

```lua
os_md5sum file.bin
```
Print the MD5 digest of `file.bin` and the file name.

## Notes

- The implementation loads each whole file into memory before hashing it.
