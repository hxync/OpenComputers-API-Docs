# adapter_api

## Summary

Internal helper API used by the devfs hardware starter to build readable and writable adapter files.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\devfs\01_hw.lua`

## Usage

```lua
-- internal devfs starter helpers passed into adapter modules
```

```lua
local starter = dofile("/lib/core/devfs/01_hw.lua")
```

## API

### adapter_api.create_toggle(read, write, switch)

- Description: Build a tiny `{read=..., write=...}` adapter that exposes boolean state as `true`/`false` text and accepts `0/1/true/false` writes.
- Parameters:
  - `read`: Optional callback used to read the current boolean-like state.
  - `write`: Callback used to write the enabled/on state.
  - `switch`: Optional callback used instead of `write(false)` when an explicit off action exists.

```lua
local toggle = adapter_api.create_toggle(proxy.isOn, proxy.turnOn, proxy.turnOff)
```

### adapter_api.createWriter(callback, ...)

- Description: Wrap one component callback so it can parse a whitespace-separated text line into typed arguments before invocation.
- Parameters:
  - `callback`: Function that should receive the parsed arguments.
  - `...`: Argument-pack declaration: required count followed by per-argument type names such as `number` or `boolean`.

```lua
local writeDepth = adapter_api.createWriter(proxy.setDepth, 1, 'number')
```

### adapter_api.make_link(list, addr, prefix, bOmitZero)

- Description: Insert a unique symlink-style entry into a listing table, choosing the first free numeric suffix for the requested prefix.
- Parameters:
  - `list`: Table that receives the generated link entry.
  - `addr`: Link target path stored in the generated entry.
  - `prefix`: Optional textual prefix for generated names.
  - `bOmitZero`: When true, the first generated name uses the bare prefix instead of suffix `0`.

```lua
adapter_api.make_link(typeDir.list, '../../by-address/' .. addr)
```

### adapter_api.toArgsPack(input, pack)

- Description: Split one input line into tokens and coerce them into a packed argument list according to a type declaration.
- Parameters:
  - `input`: Raw textual input line to parse.
  - `pack`: Type declaration table whose first element is the minimum required argument count.

```lua
local args, why = adapter_api.toArgsPack('80 25', {2, 'number', 'number'})
```

## Notes

- This page documents the exported helper table named `adapter_api`, even though the starter file itself returns the top-level `/dev/components` tree description.
