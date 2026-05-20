# transforms

## Summary

Extended sequence-manipulation helpers layered onto the base `transforms` module.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_transforms.lua`

## Usage

```lua
local transforms = require("transforms")  -- advanced helpers load lazily
```

## API

### transforms.at(tbl, index)

- Description: Return the key-value pair found at a given `pairs` iteration position.
- Parameters:
  - `tbl`: Table iterated with `pairs`.
  - `index`: 1-based pair index to retrieve.

```lua
local key, value = transforms.at(env, 1)
```

### transforms.foreach(tbl,c,f,l)

- Description: Map a callback across a table range and collect non-`nil` results.
- Parameters:
  - `tbl`: Source indexed table.
  - `c`: Callback function, or a field name whose value should be extracted from each element.
  - `f`: Optional first index.
  - `l`: Optional last index.

```lua
local names = transforms.foreach(parts, function(part) return part.txt end)
```

### transforms.partition(tbl,partitioner,dropEnds,f,l)

- Description: Split a table into subranges using a delimiter predicate or delimiter sequence.
- Parameters:
  - `tbl`: Source indexed table.
  - `partitioner`: Delimiter predicate, or a table of delimiter sequences.
  - `dropEnds`: When true, omit delimiter elements from the returned partitions.
  - `f`: Optional first index.
  - `l`: Optional last index.

```lua
local groups = transforms.partition(words, function(word) return word == '|' end, true)
```

### transforms.sub(tbl,f,l)

- Description: Return a table slice similar to `string.sub`, but for indexed table elements.
- Parameters:
  - `tbl`: Source indexed table.
  - `f`: Optional first index.
  - `l`: Optional last index.

```lua
local head = transforms.sub(words, 1, 3)
```

### transforms.where(tbl,p,f,l)

- Description: Filter a table range by predicate and return only the matching elements.
- Parameters:
  - `tbl`: Source indexed table.
  - `p`: Predicate called as `p(element, index, table)`.
  - `f`: Optional first index.
  - `l`: Optional last index.

```lua
local quoted = transforms.where(parts, function(part) return part.qr end)
```

## Notes

- Only exported module functions are listed on this page.
