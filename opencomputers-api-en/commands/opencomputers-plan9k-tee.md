# tee

## Summary

Append standard input to a file while also echoing it to standard output.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\tee.lua`

## Syntax

```lua
tee FILE
```

## Purpose

Open the target file in append mode, keep reading line-oriented data from standard input, mirror the received data to standard output, and flush the file after every chunk.

## Parameters

- `FILE`: Destination file that will receive appended data.

## Examples

```lua
dmesg | tee /kern.log
```
Mirror kernel log output to the screen while appending it to `/kern.log`.

## Notes

- This implementation always appends and does not offer a truncate mode.
