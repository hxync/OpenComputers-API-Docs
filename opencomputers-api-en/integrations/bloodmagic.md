# bloodmagic

## Summary

This page documents the Blood Magic integration exposed to Lua by OpenComputers. The available components cover blood altars and master ritual stones.

## Availability

- Dependency: `bloodmagic`
- Label: `integration-required`

## Component Names

- `blood_altar`
- `master_ritual_stone`

## What This Integration Is Good For

- Monitoring blood altar capacity, current blood volume, and altar tier.
- Reading altar progress and its sacrifice-related multipliers.
- Inspecting master ritual stone ownership, ritual state, cooldown, and running time.

## Components

### blood_altar

`blood_altar` is exposed on Blood Magic blood altars implementing `IBloodAltar`.

#### `getCapacity()`

- Syntax: `getCapacity(): number`
- Purpose: Return the altar's total blood capacity.
- Returns:
  - Maximum blood amount the altar can hold.

#### `getCurrentBlood()`

- Syntax: `getCurrentBlood(): number`
- Purpose: Return how much blood is currently stored in the altar.
- Returns:
  - Current blood amount.

#### `getTier()`

- Syntax: `getTier(): number`
- Purpose: Return the current altar tier.
- Returns:
  - Tier number.

#### `getProgress()`

- Syntax: `getProgress(): number`
- Purpose: Return the current altar operation progress.
- Returns:
  - Current progress value.
- Usage notes:
  - This is the raw value returned by `IBloodAltar.getProgress()`. The driver does not normalize it, clamp it, or convert it to a percentage.
  - In practice it is most useful for comparing consecutive readings from the same altar instead of assuming a fixed `0..100` scale.

#### `getSacrificeMultiplier()`

- Syntax: `getSacrificeMultiplier(): number`
- Purpose: Return the altar's sacrifice multiplier.
- Returns:
  - Current sacrifice multiplier.

#### `getSelfSacrificeMultiplier()`

- Syntax: `getSelfSacrificeMultiplier(): number`
- Purpose: Return the altar's self-sacrifice multiplier.
- Returns:
  - Current self-sacrifice multiplier.

#### `getOrbMultiplier()`

- Syntax: `getOrbMultiplier(): number`
- Purpose: Return the orb multiplier applied by the altar.
- Returns:
  - Current orb multiplier.

#### `getDislocationMultiplier()`

- Syntax: `getDislocationMultiplier(): number`
- Purpose: Return the altar's dislocation multiplier.
- Returns:
  - Current dislocation multiplier.

#### `getBufferCapacity()`

- Syntax: `getBufferCapacity(): number`
- Purpose: Return the altar's IO buffer capacity.
- Returns:
  - IO buffer capacity value.

### master_ritual_stone

`master_ritual_stone` is exposed on Blood Magic master ritual stones implementing `IMasterRitualStone`.

#### `getOwner()`

- Syntax: `getOwner(): string`
- Purpose: Return the player name that owns this ritual stone.
- Returns:
  - Owner name string.

#### `getCurrentRitual()`

- Syntax: `getCurrentRitual(): string`
- Purpose: Return the ritual currently associated with the stone.
- Returns:
  - Ritual identifier or name on normal Blood Magic tile implementations.
  - `"internal error"` if the backing tile does not expose the expected master stone implementation.

#### `getCooldown()`

- Syntax: `getCooldown(): number`
- Purpose: Return the remaining ritual cooldown.
- Returns:
  - Cooldown value.

#### `getRunningTime()`

- Syntax: `getRunningTime(): number`
- Purpose: Return how long the ritual has been running.
- Returns:
  - Running time value.

#### `areTanksEmpty()`

- Syntax: `areTanksEmpty(): boolean`
- Purpose: Return whether the ritual stone's tanks are empty.
- Returns:
  - `true` if the tanks are empty.
  - `false` otherwise.

## Example

```lua
local component = require("component")

if component.isAvailable("blood_altar") then
  local altar = component.blood_altar
  print("Blood:", altar.getCurrentBlood(), "/", altar.getCapacity())
  print("Tier:", altar.getTier())
end

if component.isAvailable("master_ritual_stone") then
  local stone = component.master_ritual_stone
  print("Owner:", stone.getOwner())
  print("Ritual:", stone.getCurrentRitual())
end
```

## Related

- `thaumcraft`
- `vanilla`
