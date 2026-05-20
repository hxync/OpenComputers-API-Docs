# md5sum

## Summary

Compute MD5 digests for standard input or one or more files.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\md5sum.lua`

## Syntax

```lua
md5sum [FILE...]
```

## Purpose

Read the entire input into memory, compute an MD5 digest, and print the result as hexadecimal text. When file arguments are supplied, one digest line is printed for each file.

## Parameters

- `FILE...`: Optional files to hash. If omitted, read all data from standard input.

## Examples

```lua
md5sum file.bin
```
Print the MD5 digest of `file.bin` followed by the file name.

```lua
cat file.bin | md5sum
```
Compute an MD5 digest for piped standard input.

## Notes

- The implementation reads each whole file into memory before hashing it.
