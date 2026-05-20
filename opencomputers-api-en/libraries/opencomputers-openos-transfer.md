# transfer

## Summary

Shared recursive copy and move helpers used by commands such as `cp`, `mv`, and `install`.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tools\transfer.lua`

## Usage

```lua
local transfer = dofile("/lib/tools/transfer.lua")
```

## API

### transfer.batch(args, options)

- Description: Apply one copy or move operation to a whole argument vector, treating the last argument as the destination.
- Parameters:
  - `args`: Source-and-destination argument array; the last element is the destination.
  - `options`: Normalized option table such as `{cmd='cp', r=true, v=true}`.

```lua
local exitCode = transfer.batch({'src', 'dst'}, {cmd='cp', r=true})
```

### transfer.recurse(fromPath, toPath, options, origin, top)

- Description: Copy or move one path into another, descending into directories and honoring overwrite, link, and mount rules.
- Parameters:
  - `fromPath`: Source path as written by the caller.
  - `toPath`: Destination path as written by the caller.
  - `options`: Operation options table shared across the whole transfer job.
  - `origin`: Mount-origin lookup table used to protect mount points and cross-mount traversal.
  - `top`: Whether this invocation is the top-level call rather than a recursive child call.

```lua
transfer.recurse('from', 'to', {cmd='mv', v=true}, mountOrigins, true)
```

## Notes

- The helper understands directory recursion, symbolic links, mount-point protection, interactive overwrite modes, and the standardized option tables built by shell parsers.
