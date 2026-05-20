# cat

## Summary

Write file contents or standard input to standard output.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\cat.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\cat`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\cat`

## Syntax

```lua
cat [FILE...]
```

## Purpose

Concatenate one or more files to standard output. If no operand is given, or if an operand is `-`, the command reads from standard input instead.

## Parameters

- `FILE...`: Files to print in order. Use `-` to read from standard input at that position.

## Examples

```lua
cat
```
Echo standard input back to standard output until the input stream ends.

```lua
cat /etc/motd
```
Print the contents of `/etc/motd`.

```lua
cat a.txt b.txt
```
Print `a.txt` first, then append the contents of `b.txt`.

## Notes

- Directories are rejected with an error instead of being traversed.
- If any requested file cannot be opened, the command stops immediately and exits with an error status.
