# unicode

## Summary

The `unicode` API provides codepoint-aware string helpers and display-width helpers for terminal-safe text handling.

## Availability

- Repository: `opencomputers`
- Type: core runtime API

## Source

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\UnicodeAPI.scala`

## Usage

```lua
local unicode = require("unicode")
print(unicode.len("hello"))
```

## API

### char(...codepoints)

- Description: Create a string from one or more Unicode codepoints.

### len(value)

- Description: Return the codepoint length of a string.

### lower(value)

- Description: Return a lowercase version of the string.

### reverse(value)

- Description: Return the reversed Unicode string.

### sub(value, start[, finish])

- Description: Return a substring by codepoint index.

### upper(value)

- Description: Return an uppercase version of the string.

### isWide(char)

- Description: Return whether the first codepoint is wide.

### charWidth(char)

- Description: Return the display width of the first codepoint.

### wlen(value)

- Description: Return the display width of a string.

### wtrunc(value, count)

- Description: Truncate a string to a target display width.

## Related

- `computer`
- `component`
- `os`
- `unicode`
