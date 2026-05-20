# thaumcraft

## Summary

This page documents the Thaumcraft integration exposed to Lua by OpenComputers. The available components cover the arcane crafting robot upgrade, infusion matrices, and aspect containers.

## Availability

- Dependency: `thaumcraft`
- Label: `integration-required`

## Component Names

- `arcane_crafting`
- `infusion_matrix`
- `aspect_container`

## What This Integration Is Good For

- Running vanilla or arcane crafting from a robot using a Thaumcraft upgrade.
- Monitoring infusion matrix activity, instability, and symmetry.
- Reading stored aspects from jars and other aspect containers.

## Components

### arcane_crafting

`arcane_crafting` is exposed by the Thaumcraft arcane crafting robot upgrade.

#### `craft([count])`

- Syntax: `craft([count: number]): boolean, number[, string]`
- Purpose: Try to craft items from the top-left 3x3 crafting area using vanilla recipes first, then Thaumcraft arcane recipes if needed.
- Parameters:
  - `count: number`
    Optional desired output item count. Clamped to `0` through `64`.
- Returns:
  - Success flag.
  - Number of items crafted.
  - Optional error message on failure.
- Usage notes:
  - If normal crafting succeeds, arcane crafting is not attempted.
  - Common failure messages include:
    - `Can't find the recipe.`
    - `Missing a valid Wand to craft.`
    - `Insufficient Vis in wand to perform crafting.`

#### `craftNormal([count])`

- Syntax: `craftNormal([count: number]): boolean, number[, string]`
- Purpose: Try to craft items using only vanilla crafting recipes.
- Parameters:
  - `count: number`
    Optional desired output item count, clamped to `0` through `64`.
- Returns:
  - Success flag.
  - Number of items crafted.
  - Optional error message.

#### `craftArcane([count])`

- Syntax: `craftArcane([count: number]): boolean, number[, string]`
- Purpose: Try to craft items using only Thaumcraft arcane crafting recipes.
- Parameters:
  - `count: number`
    Optional desired output item count, clamped to `0` through `64`.
- Returns:
  - Success flag.
  - Number of items crafted.
  - Optional error message.
- Usage notes:
  - The robot must have a valid wand in upgrade slot `0`.
  - The wand must contain enough Vis for the requested recipe.
  - The owner must also satisfy Thaumcraft-side requirements such as available research and recipe conditions.

### infusion_matrix

`infusion_matrix` is exposed on Thaumcraft infusion matrices.

#### `isActive()`

- Syntax: `isActive(): boolean`
- Purpose: Return whether the infusion matrix is active.
- Returns:
  - `true` if active.
  - `false` otherwise.

#### `isCrafting()`

- Syntax: `isCrafting(): boolean`
- Purpose: Return whether the infusion matrix is currently crafting.
- Returns:
  - `true` if an infusion craft is in progress.
  - `false` otherwise.

#### `getInstability()`

- Syntax: `getInstability(): number`
- Purpose: Return the current instability value.
- Returns:
  - Current instability.

#### `getSymmetry()`

- Syntax: `getSymmetry(): number`
- Purpose: Return the current symmetry value.
- Returns:
  - Current symmetry score.

### aspect_container

`aspect_container` is exposed on Thaumcraft tiles implementing `IAspectContainer`.

#### `getAspects()`

- Syntax: `getAspects(): table`
- Purpose: Return all aspects currently stored in the block.
- Returns:
  - Array of aspect tables.
- Usage notes:
  - Each returned aspect table contains:
    - `name`
    - `amount`

#### `getAspectCount(aspect)`

- Syntax: `getAspectCount(aspect: string): number`
- Purpose: Return the stored amount of one specific aspect.
- Parameters:
  - `aspect: string`
    Aspect name, matched case-insensitively after conversion to lowercase.
- Returns:
  - Stored amount for that aspect.
- Usage notes:
  - Invalid aspect names raise an argument error.

## Example

```lua
local component = require("component")

if component.isAvailable("infusion_matrix") then
  local matrix = component.infusion_matrix
  print("Active:", matrix.isActive())
  print("Instability:", matrix.getInstability())
end

if component.isAvailable("aspect_container") then
  local jar = component.aspect_container
  for _, aspect in ipairs(jar.getAspects()) do
    print(aspect.name, aspect.amount)
  end
end
```

## Related

- `thaumicenergistics`
- `bloodmagic`
