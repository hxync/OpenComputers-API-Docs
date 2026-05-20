# forestry

## Summary

This page documents the Forestry integration exposed to Lua by OpenComputers. The available components cover analyzers, bee housings, and the robot `beekeeper` upgrade.

## Availability

- Dependency: `forestry`
- Label: `integration-required`

## Component Names

- `forestry_analyzer`
- `bee_housing`
- `beekeeper`

## What This Integration Is Good For

- Reading analyzed bee data from Forestry analyzers.
- Inspecting drones, queens, and mutation data from apiaries and related bee housings.
- Using a robot upgrade to swap bees, analyze specimens, and manage GregTech industrial apiary upgrades.

## Returned Forestry Individual Data

Several callbacks on this page return Forestry individual data tables produced by the OpenComputers converter layer.

Common top-level fields include:

- `displayName`
- `ident`
- `isAnalyzed`
- `isSecret`
- `type`

Depending on the specimen, tables may also contain:

- `health`
- `maxHealth`
- `generation`
- `isAlive`
- `isNatural`
- `canSpawn`
- `hasEffect`
- `active`
- `inactive`

For bees, the `active` and `inactive` sub-tables usually include chromosome-derived values such as:

- `species`
- `speed`
- `lifespan`
- `fertility`
- `temperatureTolerance`
- `humidityTolerance`
- `nocturnal`
- `tolerantFlyer`
- `caveDwelling`
- `flowerProvider`
- `flowering`
- `effect`
- `territory`

## Components

### forestry_analyzer

`forestry_analyzer` is exposed on Forestry analyzers.

#### `isWorking()`

- Syntax: `isWorking(): boolean`
- Purpose: Return whether the analyzer currently has work available.
- Returns:
  - `true` if it can process the current specimen.
  - `false` otherwise.

#### `getProgress()`

- Syntax: `getProgress(): number`
- Purpose: Return the progress of the current analysis operation.
- Returns:
  - A progress value between `0` and `1`.
- Usage notes:
  - The driver converts Forestry's scaled progress into a normalized fraction where higher means closer to completion.

#### `getIndividualOnDisplay()`

- Syntax: `getIndividualOnDisplay(): table`
- Purpose: Return data for the individual currently shown in the analyzer display.
- Returns:
  - Forestry individual data table.
- Usage notes:
  - The returned table depends on what type of Forestry specimen is currently displayed.

### bee_housing

`bee_housing` is exposed on Forestry bee housings such as apiaries, alvearies, and compatible bee-capable machines.

#### `canBreed()`

- Syntax: `canBreed(): boolean`
- Purpose: Return whether the current housing can work and breed bees right now.
- Returns:
  - `true` if the beekeeping logic can work.
  - `false` otherwise.

#### `getDrone()`

- Syntax: `getDrone(): table`
- Purpose: Return the drone currently installed in the housing.
- Returns:
  - Forestry individual data table for the drone.
  - `nil` if there is no drone present.

#### `getQueen()`

- Syntax: `getQueen(): table`
- Purpose: Return the queen currently installed in the housing.
- Returns:
  - Forestry individual data table for the queen.
  - `nil` if there is no queen present.

#### `getBeeBreedingData()`

- Syntax: `getBeeBreedingData(): table`
- Purpose: Return known bee mutation definitions from Forestry's bee root.
- Returns:
  - A set-like collection of mutation description tables.
- Usage notes:
  - Each mutation table may include:
    - `allele1`
    - `allele2`
    - `chance`
    - `specialConditions`
    - `result`
  - This is useful when building in-game mutation lookup or planning tools.

#### `listAllSpecies()`

- Syntax: `listAllSpecies(): table`
- Purpose: Return all bee species discovered from the registered mutation outputs.
- Returns:
  - A collection of species descriptor tables.
- Usage notes:
  - Each species entry normally includes fields such as `name`, `uid`, `humidity`, and `temperature`.

#### `getBeeParents(beeName)`

- Syntax: `getBeeParents(beeName: string): table`
- Purpose: Return mutation definitions whose result matches the specified bee species.
- Parameters:
  - `beeName: string`
    Species localized name or UID, case-insensitive.
- Returns:
  - A collection of matching mutation records.
- Usage notes:
  - Matching checks both the species localized name and UID.

### beekeeper

`beekeeper` is exposed by the Forestry beekeeper robot upgrade.

#### `swapQueen(side)`

- Syntax: `swapQueen(side: number): boolean`
- Purpose: Swap the robot's selected inventory slot with the queen slot of a bee housing on the specified side.
- Parameters:
  - `side: number`
    Relative robot side containing the target housing.
- Returns:
  - `true` if a compatible housing was found and the swap happened.
  - `false` otherwise.

#### `swapDrone(side)`

- Syntax: `swapDrone(side: number): boolean`
- Purpose: Swap the robot's selected inventory slot with the drone slot of a bee housing on the specified side.
- Parameters:
  - `side: number`
    Relative robot side containing the target housing.
- Returns:
  - `true` if a compatible housing was found and the swap happened.
  - `false` otherwise.

#### `getBeeProgress(side)`

- Syntax: `getBeeProgress(side: number): number[, string]`
- Purpose: Return the current bee work progress percent for the housing on the specified side.
- Parameters:
  - `side: number`
    Relative robot side containing the target housing.
- Returns:
  - Current progress percent on success.
  - `false, "No bee housing found"` if no compatible housing is present.

#### `canWork(side)`

- Syntax: `canWork(side: number): boolean[, string]`
- Purpose: Return whether the current bee housing can work right now.
- Parameters:
  - `side: number`
    Relative robot side containing the target housing.
- Returns:
  - `true` or `false` on success.
  - `false, "No bee housing found"` if none is present.

#### `analyze(honeySlot)`

- Syntax: `analyze(honeySlot: number): boolean[, string]`
- Purpose: Analyze the bee in the robot's selected slot, consuming one honey item from the specified slot.
- Parameters:
  - `honeySlot: number`
    Robot inventory slot containing `honeydew` or `honeyDrop`.
- Returns:
  - `true` on success.
  - `false, "Not a bee"` if the selected slot does not contain a bee.
  - `false, "No honey!"` if the specified honey slot is missing a valid honey item.
- Usage notes:
  - The selected specimen slot is the robot's currently selected inventory slot.

#### `addIndustrialUpgrade(side[, amount])`

- Syntax: `addIndustrialUpgrade(side: number[, amount: number]): number`
- Purpose: Push industrial apiary upgrades from the robot's selected slot into a GregTech industrial apiary.
- Parameters:
  - `side: number`
    Relative robot side containing the target industrial apiary.
  - `amount: number`
    Optional number of upgrades to try to install. Defaults to `64`.
- Returns:
  - Number of upgrades successfully inserted.
- Usage notes:
  - Returns `0` if no GregTech industrial apiary is found, the selected slot is empty, or nothing could be inserted.

#### `getIndustrialUpgrade(side, slot)`

- Syntax: `getIndustrialUpgrade(side: number, slot: number): table[, string]`
- Purpose: Return the upgrade stack installed in one GregTech industrial apiary upgrade slot.
- Parameters:
  - `side: number`
    Relative robot side containing the target industrial apiary.
  - `slot: number`
    One-based industrial upgrade slot index.
- Returns:
  - Item stack description table for the upgrade in that slot.
  - `nil, "Wrong slot index (should be 1-X)"` if the slot number is invalid.

#### `removeIndustrialUpgrade(side, slot[, amount])`

- Syntax: `removeIndustrialUpgrade(side: number, slot: number[, amount: number]): boolean|number[, string]`
- Purpose: Remove upgrades from a GregTech industrial apiary and place them into the robot inventory.
- Parameters:
  - `side: number`
    Relative robot side containing the target industrial apiary.
  - `slot: number`
    One-based industrial upgrade slot index.
  - `amount: number`
    Optional number of upgrades to remove. Defaults to `64`.
- Returns:
  - Number of upgrades successfully moved into the robot inventory.
  - `false, "Wrong slot index (should be 1-X)"` if the slot number is invalid.
- Usage notes:
  - The helper tries the selected robot slot first, then matching stacks, then any free slots.

## Example

```lua
local component = require("component")

if component.isAvailable("bee_housing") then
  local housing = component.bee_housing
  local queen = housing.getQueen()
  if queen then
    print("Queen:", queen.displayName, queen.isAnalyzed)
  end
end

if component.isAvailable("beekeeper") then
  local keeper = component.beekeeper
  print("Can work:", keeper.canWork(3))
end
```

## Related

- `gregtech`
- `vanilla`
