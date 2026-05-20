# ic2

## Summary

This page documents the IndustrialCraft 2 integrations exposed by OpenComputers. The integration covers crop tiles, EU network tiles, the mass fabricator, teleporters, and several reactor access points.

## Availability

- Dependency: `ic2`
- Label: `integration-required`

## Component Names

- `mass_fab`
- `crop`
- `energy_conductor`
- `energy_sink`
- `energy_source`
- `energy_storage`
- `ic2_teleporter`
- `reactor`
- `reactor_chamber`
- `reactor_redstone_port`

## What This Integration Is Good For

- Reading IC2 crop stats and environment values from Lua.
- Inspecting EU storage and conductor limits.
- Monitoring or toggling reactors through direct machine access, chambers, or redstone ports.
- Updating teleporter destinations from automation scripts.
- Tracking mass fabricator charge progress.

## Components

### mass_fab

#### getProgress()

- Syntax: `getProgress(): number`
- Purpose: Return the mass fabricator progress value used by the OC driver.
- Returns:
  - `100 * storedEnergy / demandedEnergy`.
- Usage notes:
  - This is a computed percentage-like value, not a separate IC2 field.
  - The driver does not clamp or normalize it beyond that formula.

### crop

`crop` is exposed on IC2 crop tiles.

#### getSize()

- Syntax: `getSize(): number`
- Purpose: Return the crop's current size stage.

#### getGrowth()

- Syntax: `getGrowth(): number`
- Purpose: Return the crop's growth stat.

#### getGain()

- Syntax: `getGain(): number`
- Purpose: Return the crop's gain stat.

#### getResistance()

- Syntax: `getResistance(): number`
- Purpose: Return the crop's resistance stat.

#### getNutrientStorage()

- Syntax: `getNutrientStorage(): number`
- Purpose: Return the internally stored nutrient value.

#### getHydrationStorage()

- Syntax: `getHydrationStorage(): number`
- Purpose: Return the internally stored hydration value.

#### getWeedExStorage()

- Syntax: `getWeedExStorage(): number`
- Purpose: Return the stored Weed-EX value.

#### getHumidity()

- Syntax: `getHumidity(): number`
- Purpose: Return the current humidity value around the crop.

#### getNutrients()

- Syntax: `getNutrients(): number`
- Purpose: Return the current nutrient value around the crop.

#### getAirQuality()

- Syntax: `getAirQuality(): number`
- Purpose: Return the current air-quality value around the crop.

#### getName()

- Syntax: `getName(): string`
- Purpose: Return the planted crop card name.
- Usage notes:
  - The driver reads `tileEntity.getCrop().name()` directly.

#### getRootsLength()

- Syntax: `getRootsLength(): number`
- Purpose: Return the root length reported by the planted crop card.

#### getTier()

- Syntax: `getTier(): number`
- Purpose: Return the tier reported by the planted crop card.

#### maxSize()

- Syntax: `maxSize(): number`
- Purpose: Return the maximum size of the planted crop card.

#### canGrow()

- Syntax: `canGrow(): boolean`
- Purpose: Return whether the planted crop card currently allows further growth on this tile.

#### getOptimalHavestSize()

- Syntax: `getOptimalHavestSize(): number`
- Purpose: Return the crop card's optimal harvest size.
- Usage notes:
  - The method name is spelled `Havest` in the exposed API because that is the name used by the driver.

#### Usage notes

- The crop callbacks are read-only.
- Some values come from the tile itself, while `getName()`, `getRootsLength()`, `getTier()`, `maxSize()`, `canGrow()`, and `getOptimalHavestSize()` come from the planted crop card.

### energy_conductor

#### getConductionLoss()

- Syntax: `getConductionLoss(): number`
- Purpose: Return the conductor's line loss value.

#### getConductorBreakdownEnergy()

- Syntax: `getConductorBreakdownEnergy(): number`
- Purpose: Return the conductor breakdown threshold.

#### getInsulationBreakdownEnergy()

- Syntax: `getInsulationBreakdownEnergy(): number`
- Purpose: Return the insulation breakdown threshold.

#### getInsulationEnergyAbsorption()

- Syntax: `getInsulationEnergyAbsorption(): number`
- Purpose: Return how much energy the insulation can absorb.

### energy_sink

#### getSinkTier()

- Syntax: `getSinkTier(): number`
- Purpose: Return the IC2 sink tier accepted by this tile.

### energy_source

#### getOfferedEnergy()

- Syntax: `getOfferedEnergy(): number`
- Purpose: Return how much energy the source currently offers.

### energy_storage

#### getCapacity()

- Syntax: `getCapacity(): number`
- Purpose: Return the maximum stored energy capacity.

#### getOutput()

- Syntax: `getOutput(): number`
- Purpose: Return the current output limit.

#### getStored()

- Syntax: `getStored(): number`
- Purpose: Return the current stored energy amount.

### ic2_teleporter

#### setCoords(x, y, z)

- Syntax: `setCoords(x: number, y: number, z: number)`
- Purpose: Set the teleporter destination coordinates.
- Parameters:
  - `x`, `y`, `z`: destination coordinates.
- Returns:
  - No useful return values.
- Usage notes:
  - The destination is only updated if all three arguments are integers.
  - If one or more arguments are not integers, the callback simply does nothing.

### reactor

`reactor`, `reactor_chamber`, and `reactor_redstone_port` expose mostly the same monitoring API. The differences are in how they resolve the actual target reactor and what they do when no valid reactor is attached.

#### Shared slot info table

When a slot contains an item, `getSlotInfo(x, y)` returns a table with:

- `item`: the raw item stack.
- `canStoreHeat`: present only for reactor components.
- `heat`: present only for reactor components that track heat.
- `maxHeat`: present only for reactor components that track heat.

If the slot is empty, the callback returns `nil`.

#### reactor.getHeat()

- Syntax: `getHeat(): number`
- Purpose: Return the current reactor heat.

#### reactor.getMaxHeat()

- Syntax: `getMaxHeat(): number`
- Purpose: Return the maximum heat before meltdown.

#### reactor.getReactorEnergyOutput()

- Syntax: `getReactorEnergyOutput(): number`
- Purpose: Return the raw reactor energy output, before multiplying by the base EU/t value.

#### reactor.getReactorEUOutput()

- Syntax: `getReactorEUOutput(): number`
- Purpose: Return the base EU/t output.

#### reactor.producesEnergy()

- Syntax: `producesEnergy(): boolean`
- Purpose: Return whether the reactor is active and meant to produce energy.

#### reactor.setActive(active)

- Syntax: `setActive(active: boolean): boolean`
- Purpose: Enable or disable the reactor through its redstone-controlled state.
- Returns:
  - The applied redstone state after the write.
  - `false` if no electric reactor instance is available.

#### reactor.getSlotInfo(x, y)

- Syntax: `getSlotInfo(x: number, y: number): table | nil`
- Purpose: Return information about the specified reactor grid slot.

### reactor_chamber

`reactor_chamber` forwards calls through an attached reactor chamber.

#### reactor_chamber.getHeat()

- Syntax: `getHeat(): number`
- Purpose: Return attached reactor heat.
- Returns:
  - The current heat.
  - `0` if the chamber is not attached to a valid reactor.

#### reactor_chamber.getMaxHeat()

- Syntax: `getMaxHeat(): number`
- Purpose: Return attached reactor maximum heat.
- Returns:
  - The reactor maximum heat.
  - `0` if the chamber is not attached to a valid reactor.

#### reactor_chamber.getReactorEnergyOutput()

- Syntax: `getReactorEnergyOutput(): number`
- Purpose: Return attached reactor raw energy output.
- Returns:
  - The raw output.
  - `0` if the chamber is not attached to a valid reactor.

#### reactor_chamber.getReactorEUOutput()

- Syntax: `getReactorEUOutput(): number`
- Purpose: Return attached reactor base EU/t output.
- Usage notes:
  - Unlike several other chamber callbacks, the driver does not guard this call with a null fallback before reading the value.

#### reactor_chamber.producesEnergy()

- Syntax: `producesEnergy(): boolean`
- Purpose: Return whether the attached reactor is producing energy.
- Returns:
  - The live reactor state.
  - `false` if the chamber is not attached to a valid reactor.

#### reactor_chamber.setActive(active)

- Syntax: `setActive(active: boolean): boolean`
- Purpose: Toggle the attached reactor through its redstone state.
- Returns:
  - The applied redstone state.
  - `false` if no electric reactor instance is available.

#### reactor_chamber.getSlotInfo(x, y)

- Syntax: `getSlotInfo(x: number, y: number): table | nil`
- Purpose: Return information about the specified reactor slot through the attached chamber.
- Returns:
  - The shared slot info table when an item is present.
  - `nil` if the chamber has no reactor or the target slot is empty.

### reactor_redstone_port

`reactor_redstone_port` resolves the attached reactor or reactor chamber internally, then exposes the same monitoring API plus one extra heat callback.

#### reactor_redstone_port.getHeat()

- Syntax: `getHeat(): number`
- Purpose: Return reactor heat.
- Returns:
  - The current heat.
  - `0` if no valid target reactor is available.

#### reactor_redstone_port.getMaxHeat()

- Syntax: `getMaxHeat(): number`
- Purpose: Return reactor maximum heat.
- Returns:
  - The reactor maximum heat.
  - `0` if no valid target reactor is available.

#### reactor_redstone_port.getReactorEnergyOutput()

- Syntax: `getReactorEnergyOutput(): number`
- Purpose: Return raw reactor energy output.
- Returns:
  - The raw output.
  - `0` if no valid target reactor is available.

#### reactor_redstone_port.getReactorEUOutput()

- Syntax: `getReactorEUOutput(): number`
- Purpose: Return base EU/t output from the resolved target reactor.
- Usage notes:
  - As with the chamber variant, the driver reads the value directly after resolving the target and does not document a `0` fallback for this specific method.

#### reactor_redstone_port.producesEnergy()

- Syntax: `producesEnergy(): boolean`
- Purpose: Return whether the resolved target reactor is producing energy.
- Returns:
  - The live reactor state.
  - `false` if no valid target reactor is available.

#### reactor_redstone_port.setActive(active)

- Syntax: `setActive(active: boolean): boolean`
- Purpose: Toggle the attached reactor through its redstone state.
- Returns:
  - The applied redstone state.
  - `false` if no electric reactor instance is available.

#### reactor_redstone_port.getEmitHeat()

- Syntax: `getEmitHeat(): number`
- Purpose: Return emitted heat, which is especially useful for fluid-reactor setups.
- Returns:
  - `EmitHeat` from the resolved electric reactor.
  - `0` if the resolved target is not an electric reactor.

#### reactor_redstone_port.getSlotInfo(x, y)

- Syntax: `getSlotInfo(x: number, y: number): table | nil`
- Purpose: Return information about the specified reactor slot through the redstone port.
- Returns:
  - The shared slot info table when an item is present.
  - `nil` if no target reactor exists or the target slot is empty.

## Example

```lua
local component = require("component")

if component.isAvailable("crop") then
  local crop = component.crop
  print("Crop:", crop.getName(), "size", crop.getSize(), "/", crop.maxSize())
  print("Can grow:", crop.canGrow())
end

if component.isAvailable("reactor") then
  local reactor = component.reactor
  print("Heat:", reactor.getHeat(), "/", reactor.getMaxHeat())
  print("EU/t:", reactor.getReactorEUOutput())
end
```

## Related

- `gregtech`
- `mekanism`
