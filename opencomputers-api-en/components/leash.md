# leash

## Summary

The `leash` component is the OpenComputers leash upgrade. It can leash nearby living entities to the host and later release all entities currently leashed by that upgrade.

## Availability

- Repository: `opencomputers`
- Component name: `leash`
- Typical host: compatible OpenComputers upgrade host carrying the leash upgrade

## Usage

```lua
local component = require("component")
local leash = component.leash
```

## Limits

- Maximum tracked leashed entities: `8`
- The upgrade only leashes entities that return `allowLeashing() == true`.
- On disconnect, unload, or explicit `unleash()`, all tracked nearby entities are released.

## API

### leash

- Syntax: `leash.leash(side)`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Searches the specified side for the first leashable living entity and leashes it to the host.

Parameters:

- `side:number`: any valid OC side value.

Behavior:

- The search area spans from the host bounds toward the selected side by two blocks.
- On success the context pauses for `0.1` seconds.

Common failures:

- `nil, "too many leashed entities"`
- `nil, "no unleashed entity"`

Example:

```lua
local ok, reason = leash.leash(sides.front)
if not ok then
  print(reason)
end
```

### unleash

- Syntax: `leash.unleash()`
- Returns: no meaningful value
- Purpose: Releases all entities currently tracked by this upgrade.

Behavior:

- The implementation searches a `5` block expanded area around the host and clears leashes that were attached by this upgrade.

## Related

- `component`
- `drone`
