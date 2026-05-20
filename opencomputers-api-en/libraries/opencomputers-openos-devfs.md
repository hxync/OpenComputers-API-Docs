# devfs

## Summary

Virtual device-filesystem helpers that build and expose the `/dev` tree in OpenOS.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\devfs.lua`

## Usage

```lua
local devfs = require("devfs")
```

## API

### devfs.create(path, proxy)

- Description: Create one virtual device node or directory entry under the devfs tree.
- Parameters:
  - `path`: Virtual path relative to the devfs root, such as `/components/by-type`.
  - `proxy`: Proxy table for the new node, or `nil` to create an empty directory container.

```lua
local node, reason = devfs.create("/custom/sensor", {read = function() return "42" end})
```

### devfs.getDevice(path)

- Description: Resolve a path back to the underlying device proxy, whether it points at `/dev/...` or a mounted filesystem entry.
- Parameters:
  - `path`: Absolute or shell-resolved path to inspect.

```lua
local proxy, reason = devfs.getDevice("/dev/components/by-type/gpu")
```

### devfs.register(public_proxy)

- Description: Load starter modules from `/lib/core/devfs`, populate the tree once, and hook dynamic child discovery into the mounted filesystem proxy.
- Parameters:
  - `public_proxy`: Mounted filesystem proxy table for `/dev`, used to inject dynamic directory children when available.

```lua
devfs.register(fsProxy)
```

## Notes

- The public module focuses on building device nodes and resolving them back to component or filesystem proxies.
- Internal filesystem proxy methods such as `api.proxy.open` are used by the mounted `/dev` filesystem and are intentionally kept off this page.
