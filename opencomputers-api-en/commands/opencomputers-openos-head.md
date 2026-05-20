# head

## Summary

Print the first lines or bytes from files or standard input.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\head.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\head`

## Syntax

```lua
head [OPTION]... [FILE]...
```

## Purpose

Read the beginning of each input source. By default, `head` prints the first `10` lines, but it can switch to byte-count mode, print all but the last `n` items when a leading minus is used, and control per-file headers.

## Parameters

- `FILE...`: Files to read. Use `-` or omit all files to read from standard input.

## Options

- `--bytes=[-]n`: Print the first `n` bytes, or all but the last `n` bytes when `n` is prefixed with `-`.
- `--lines=[-]n`: Print the first `n` lines, or all but the last `n` lines when `n` is prefixed with `-`.
- `-q, --quiet, --silent`: Never print file-name headers when reading multiple files.
- `-v, --verbose`: Always print file-name headers.
- `--help`: Print the help message and exit.

## Examples

```lua
head -
```
Read the first 10 lines from standard input and then stop.

```lua
head file.txt
```
Print the first 10 lines of `file.txt`.

```lua
head --lines=32 file.txt
```
Print the first 32 lines of `file.txt`.
