# wc

## Summary

Count bytes, characters, lines, or words from standard input.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\wc.lua`

## Syntax

```lua
wc
```
```lua
wc -c
```
```lua
wc -m
```
```lua
wc -l
```

## Purpose

Read data from the current standard input stream and print one count. By default the command prints the word count; options switch the output to bytes, Unicode characters, or lines.

## Options

- `-c, --bytes`: Print the byte count.
- `-m, --chars`: Print the Unicode character count.
- `-l, --lines`: Print the line count.

## Examples

```lua
echo hello | wc
```
Print the default word count for the piped input.

```lua
cat /etc/motd | wc -l
```
Count how many lines are produced by the input stream.

## Notes

- This Plan9k implementation reads only from standard input and does not accept file path arguments directly.
