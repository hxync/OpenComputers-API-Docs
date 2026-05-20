# text

## Summary

String tokenization, wrapping, padding, and escape helpers for shell- and terminal-oriented text.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\text.lua`

## Usage

```lua
local text = require("text")
```

## API

### text.detab(value, tabWidth)

- Description: Replace tab characters with spaces using the supplied or default tab width.
- Parameters:
  - `value`
  - `tabWidth`

```lua
text.detab(nil, nil)
```

### text.padLeft(value, length)

- Description: Left-pad a string with spaces until it reaches the requested display width.
- Parameters:
  - `value`
  - `length`

```lua
text.padLeft(nil, nil)
```

### text.padRight(value, length)

- Description: Right-pad a string with spaces until it reaches the requested display width.
- Parameters:
  - `value`
  - `length`

```lua
text.padRight(nil, nil)
```

### text.tokenize(value)

- Description: Split shell-like input into normalized tokens while respecting quotes and escapes.
- Parameters:
  - `value`

```lua
text.tokenize(nil)
```

### text.trim(value)

- Description: Remove leading and trailing whitespace from a string.
- Parameters:
  - `value`

```lua
text.trim(nil)
```

### text.wrap(value, width, maxWidth)

- Description: Take the next display-width-bounded line fragment from a string and return the remainder.
- Parameters:
  - `value`
  - `width`
  - `maxWidth`

```lua
text.wrap(nil, nil, nil)
```

### text.wrappedLines(value, width, maxWidth)

- Description: Return an iterator that repeatedly applies `wrap` to produce wrapped lines.
- Parameters:
  - `value`
  - `width`
  - `maxWidth`

```lua
text.wrappedLines(nil, nil, nil)
```

## Notes

- OpenOS exposes a small bootstrap subset first and delays the richer tokenization and wrapping helpers until the full module loads.
