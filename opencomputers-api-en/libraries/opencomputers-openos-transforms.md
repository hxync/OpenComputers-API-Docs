# transforms

## Summary

Table-slice and token-sequence helpers used heavily by the OpenOS shell parser.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\transforms.lua`

## Usage

```lua
local transforms = require("transforms")
```

## API

### transforms.begins(tbl,v,f,l)

- Description: Return whether a table range begins with another indexed sequence.
- Parameters:
  - `tbl`: Source indexed table to inspect.
  - `v`: Indexed sequence that should match the beginning of the range.
  - `f`: Optional first index of the range to test.
  - `l`: Optional last index of the range to test.

```lua
transforms.begins(tokens, {'--'})
```

### transforms.concat(...)

- Description: Concatenate multiple indexed tables into one result table, preserving a packed `n` field when present.
- Parameters:
  - `...`: Any number of indexed tables to append in order.

```lua
local merged = transforms.concat(args, extra)
```

### transforms.first(tbl,pred,f,l)

- Description: Find the first element or subrange that satisfies a predicate or one of several candidate token sequences.
- Parameters:
  - `tbl`: Indexed table to scan.
  - `pred`: Predicate function, or a table of candidate indexed sequences.
  - `f`: Optional first index to scan.
  - `l`: Optional last index to scan.

```lua
local i = transforms.first(words, function(part) return part == '|' end)
```

## Notes

- The base module exposes a small core; additional helpers such as `partition` and `foreach` are attached lazily from `full_transforms`.
