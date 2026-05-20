# particle

## Summary

The `particle` component is the Computronics particle emitter card. It spawns particle effects at positions relative to the host device and optionally gives them randomized or explicit velocity.

## Availability

- Repository: `computronics`
- Component name: `particle`
- Typical host: Computronics particle card in a supported OC host

## Usage

```lua
local component = require("component")
local particle = component.particle
```

## Range And Energy

- Default maximum allowed range from the host is `Config.FX_RANGE`, which is `256` by default.
- If `FX_RANGE` is negative, the source disables the distance check.
- Energy cost is `Config.FX_ENERGY_COST * distance`.
- The default energy coefficient in the source is `0.2`.

## API

### spawn

- Syntax:
  - `particle.spawn(name, x, y, z[, defaultVelocity])`
  - `particle.spawn(name, x, y, z[, vx, vy, vz])`
- Returns:
  - Success: `true`
  - Failure: `false` or `false, reason`
- Purpose: Spawns one particle effect at coordinates relative to the host.

Parameters:

- `name:string`: particle name.
- `x:number`, `y:number`, `z:number`: relative offsets from the host position.
- `defaultVelocity:number` optional: gaussian spread scale used to generate randomized velocity.
- `vx:number`, `vy:number`, `vz:number` optional: explicit velocity components.

Behavior:

- The particle name may not exceed `Short.MAX_VALUE` characters.
- Position offsets are clamped to `-65536` through `65536`.
- Distance is computed from the relative offset vector.
- If the distance exceeds the configured maximum, the callback returns `false, "out of range"`.
- If the name is too long, it returns `false, "name too long"`.
- If the connector cannot pay the energy cost, it returns `false`.
- With only `defaultVelocity`, the final velocity is `defaultVelocity * gaussianRandom()` for each axis.
- With all three velocity components supplied, those exact values are used.

Example:

```lua
assert(particle.spawn("smoke", 0, 1, 0, 0.02))
assert(particle.spawn("reddust", 0, 1, 0, 0, 0.05, 0))
```

## Related

- `component`
- `computronics-computronics-explode`
