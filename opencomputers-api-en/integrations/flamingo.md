# flamingo

## Summary

This page documents the small but fun OpenComputers integration for the Flamingo mod.

## Availability

- Dependency: `flamingo`
- Label: `integration-required`

## Component Name

- `flamingo`

## What This Integration Is Good For

- Triggering decorative motion from Lua programs.
- Building novelty exhibits and event-driven displays.
- Synchronizing physical props with timers or external control signals.

## Component

### flamingo

The `flamingo` component is exposed on Flamingo tile entities.

#### `wiggle()`

- Syntax: `wiggle()`
- Purpose: Trigger the flamingo's wiggle animation.
- Returns:
  - This callback does not return a meaningful value.
- Usage notes:
  - The animation is triggered through the block event system, so it is intended for visible in-world feedback rather than data processing.

#### `getWiggleStrength()`

- Syntax: `getWiggleStrength(): number`
- Purpose: Return the current wiggle strength value stored by the tile.
- Returns:
  - The current stored wiggle strength.
- Usage notes:
  - This is useful when you want to decide whether another wiggle should be triggered.

#### Example

```lua
local component = require("component")
local flamingo = component.flamingo

print("Current wiggle strength:", flamingo.getWiggleStrength())
flamingo.wiggle()
```

## Related

- `armourersworkshop`
