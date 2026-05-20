# dd

## Summary

Copy raw data between files or standard streams in fixed-size blocks.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\dd.lua`

## Syntax

```lua
dd [if=INPUT] [of=OUTPUT] [bs=BYTES] [count=BLOCKS] [wait=SECONDS]
```

## Purpose

Parse every argument as a `key=value` assignment, then copy data from the input source to the output source. The command can read from standard input, write to standard output, limit the number of blocks, delay between blocks, and print a final transfer summary.

## Parameters

- `if=INPUT`: Input file path. Use `-` or omit it to read from standard input.
- `of=OUTPUT`: Output file path. Use `-` or omit it to write to standard output.
- `bs=BYTES`: Block size in bytes. The source defaults to `128`.
- `count=BLOCKS`: Maximum number of blocks to copy. The default is unlimited until end of input.
- `wait=SECONDS`: Optional delay inserted after each copied block.

## Examples

```lua
dd if=/tmp/a.bin of=/tmp/b.bin bs=512
```
Copy `/tmp/a.bin` to `/tmp/b.bin` in 512-byte blocks.

```lua
cat image.bin | dd of=/tmp/image.copy bs=1024
```
Read binary data from standard input and store it in `/tmp/image.copy`.

```lua
dd if=/tmp/a.bin of=- count=4 bs=256
```
Write the first four 256-byte blocks of `/tmp/a.bin` to standard output.

## Notes

- All arguments must use the `key=value` form; a plain positional argument is rejected as illegal.
- The printed byte counter assumes each successful write consumed a full block, so the final total may over-report the last partial block at end of input.
