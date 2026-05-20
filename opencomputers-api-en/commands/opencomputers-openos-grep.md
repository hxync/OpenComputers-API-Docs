# grep

## Summary

Search input lines for Lua-pattern matches across files, standard input, or recursive directory scans.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\grep.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\grep`

## Syntax

```lua
grep [OPTIONS] PATTERN [FILE...]
```

## Purpose

Search each input line against one or more patterns and print matching results with optional filenames, line numbers, colors, counts, or recursive traversal. Unlike POSIX grep, this implementation uses Lua patterns by default unless fixed-string mode is selected.

## Parameters

- `PATTERN`: Primary search pattern. In default mode it is interpreted as a Lua pattern.
- `FILE...`: Optional files to search. Use `-` for standard input. If omitted, search `-`, or `.` when `-r` is used.

## Options

- `-F, --fixed-strings`: Treat the pattern as a plain literal string.
- `-e, --lua-regexp`: Keep Lua-pattern matching enabled even when fixed-string mode would otherwise be chosen.
- `--file=PATTERN_FILE`: Read one pattern per line from `PATTERN_FILE`.
- `-w, --word-regexp`: Require the match to be surrounded by non-word boundaries according to the implementation's `%a_` test.
- `-x, --line-regexp`: Require the entire line to match the pattern.
- `-i, --ignore-case`: Build a case-insensitive Lua pattern from each supplied pattern.
- `--label=NAME`: Use `NAME` when standard input is displayed as a file label.
- `-s, --no-messages`: Suppress file-read diagnostic messages.
- `-v, --invert-match`: Select non-matching lines instead of matching lines.
- `--max-count=N`: Stop after `N` matches in the current input stream.
- `-n, --line-number`: Print line numbers.
- `-r, --recursive`: Recursively gather files from directory arguments.
- `-l, --files-with-matches`: Print only file names that contain at least one match.
- `-L, --files-without-match`: Print only file names that contain no matches.
- `-H, --with-filename`: Always show file names in output.
- `-h, --no-filename`: Suppress file names in output.
- `-o, --only-matching`: Print only the matching fragment instead of the whole line.
- `-q, --quiet, --silent`: Suppress normal output and use the exit code to signal whether any match was found.
- `-c, --count`: Print the number of matches per input source.
- `--color, --colour`: Colorize matches when writing to a terminal.
- `-t, --trim`: Trim leading and trailing whitespace around printed matches and lines.
- `-V, --version, --help`: Print the short usage text.

## Examples

```lua
grep hello notes.txt
```
Search `notes.txt` for lines matching the Lua pattern `hello`.

```lua
grep -i -n 'error' /var/log.txt
```
Search case-insensitively and print matching line numbers.

```lua
grep -r -l 'TODO' /home/project
```
Recursively list only files under `/home/project` that contain at least one `TODO` match.

## Notes

- Recursive mode gathers only files; directories themselves are not searched as records.
- Exit status is `0` when any match is found, `1` when no match is found, and `2` for usage or file errors.
