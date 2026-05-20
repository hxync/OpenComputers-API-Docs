# sha256sum

## Summary

Compute SHA-256 digests for standard input or one or more files.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\sha256sum.lua`

## Syntax

```lua
sha256sum [FILE...]
```

## Purpose

Read the entire input into memory, compute a SHA-256 digest, and print the result as hexadecimal text. When file arguments are supplied, one digest line is printed for each file.

## Parameters

- `FILE...`: Optional files to hash. If omitted, read all data from standard input.

## Examples

```lua
sha256sum file.bin
```
Print the SHA-256 digest of `file.bin` followed by the file name.

```lua
cat file.bin | sha256sum
```
Compute a SHA-256 digest for piped standard input.

## Notes

- The implementation reads each whole file into memory before hashing it.
