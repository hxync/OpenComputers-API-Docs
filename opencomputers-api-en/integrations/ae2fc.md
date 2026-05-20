# ae2fc

## Summary

This page documents the AE2 Fluid Craft integration exposed to Lua by OpenComputers. It extends the Applied Energistics-style API with fluid export buses, fluid import buses, fluid storage buses, and fluid interfaces.

## Availability

- Dependency: `ae2fc`
- Label: `integration-required`

## Component Names

- `fluid_exportbus`
- `fluid_importbus`
- `fluid_storagebus`
- `fluid_interface`

## What This Integration Is Good For

- Reading and changing fluid filter slots on AE2 Fluid Craft buses.
- Configuring fluid interfaces with fluid stack descriptors.
- Reusing the familiar AE2 bus workflow while working with fluids instead of items.
- Building fluid automation that matches fluids by registry name, amount, and optional stack size.

## Fluid Detail Tables

Most write callbacks on this page accept a fluid detail table. These tables are parsed through the same converter used for OpenComputers fluid stacks.

Useful fields are:

- `name: string`
  Fluid registry name such as `water`, `lava`, or a modded fluid id.
- `id: number`
  Optional numeric fluid id. You can use this instead of `name`.
- `amount: number`
  Fluid amount. If omitted during parsing, it defaults to `0`.
- `size: number`
  Optional AE stack size. If omitted, it defaults to `1`.

Usage notes:

- You must provide either `name` or `id`, otherwise the parser throws `fluid id or name expected`.
- If the supplied fluid id or fluid name does not exist, the parser throws `fluid not found`.
- Passing no detail table clears the selected configuration slot.
- Returned tables are produced by the OC fluid converter plus the AE stack wrapper and normally include:
  - `name: string`, the fluid registry name.
  - `label: string`, the localized fluid name.
  - `amount: number`, the fluid amount.
  - `hasTag: boolean`, whether the backing `FluidStack` carries NBT.
  - `size: number`, the AE stack size, defaulting to `1` if not set explicitly.
  - `isCraftable: boolean`, whether the AE stack is marked craftable.
- The numeric `id: number` field is only present when `insertIdsInConverters` is enabled in OC's configuration.

## Components

### fluid_exportbus

`fluid_exportbus` is exposed on AE2 Fluid Craft fluid export bus parts.

#### `getExportConfiguration(side[, slot])`

- Syntax: `getExportConfiguration(side: number[, slot: number]): table`
- Purpose: Return the configured fluid descriptor in one export slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid export bus.
  - `slot: number`
    Optional configuration slot index.
- Returns:
  - A fluid descriptor table for the selected slot.
- Usage notes:
  - If `slot` is omitted, slot `0` is used.
  - Invalid slot indices throw `invalid slot`.
  - Returned tables follow the fluid detail table shape described above.

#### `setExportConfiguration(side[, slot][, detail])`

- Syntax: `setExportConfiguration(side: number[, slot: number][, detail: table]): boolean`
- Purpose: Set or clear one export filter slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid export bus.
  - `slot: number`
    Optional configuration slot index.
  - `detail: table`
    Optional fluid descriptor table.
- Returns:
  - `true` on success.
- Usage notes:
  - Omitting `detail` clears the selected slot.
  - Invalid slot indices throw `invalid slot`.
  - This callback accepts fluid descriptors, even though the inherited helper name still references item buses internally.

#### `getExportSlotSize(side)`

- Syntax: `getExportSlotSize(side: number): number`
- Purpose: Return how many configuration slots are available on the bus.
- Parameters:
  - `side: number`
    Multipart side containing the fluid export bus.
- Returns:
  - Number of valid config slots.

#### `getExportOreFilter(side)`

- Syntax: `getExportOreFilter(side: number): string`
- Purpose: Return the ore-dictionary style filter string used by the bus.
- Parameters:
  - `side: number`
    Multipart side containing the fluid export bus.
- Returns:
  - Current filter string.
- Usage notes:
  - The API still uses the legacy AE bus filter naming, even on fluid buses.

#### `setExportOreFilter(side, filter)`

- Syntax: `setExportOreFilter(side: number, filter: string): boolean`
- Purpose: Change the bus filter string.
- Parameters:
  - `side: number`
    Multipart side containing the fluid export bus.
  - `filter: string`
    New filter string.
- Returns:
  - `true` on success.

### fluid_importbus

`fluid_importbus` is exposed on AE2 Fluid Craft fluid import bus parts.

#### `getImportConfiguration(side[, slot])`

- Syntax: `getImportConfiguration(side: number[, slot: number]): table`
- Purpose: Return the configured fluid descriptor in one import slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid import bus.
  - `slot: number`
    Optional configuration slot index.
- Returns:
  - A fluid descriptor table.
- Usage notes:
  - If `slot` is omitted, slot `0` is used.
  - Invalid slot indices throw `invalid slot`.

#### `setImportConfiguration(side[, slot][, detail])`

- Syntax: `setImportConfiguration(side: number[, slot: number][, detail: table]): boolean`
- Purpose: Set or clear one import filter slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid import bus.
  - `slot: number`
    Optional configuration slot index.
  - `detail: table`
    Optional fluid descriptor table.
- Returns:
  - `true` on success.
- Usage notes:
  - Omitting `detail` clears the selected slot.
  - Invalid slot indices throw `invalid slot`.

#### `getImportSlotSize(side)`

- Syntax: `getImportSlotSize(side: number): number`
- Purpose: Return the number of valid import configuration slots.
- Parameters:
  - `side: number`
    Multipart side containing the fluid import bus.
- Returns:
  - Slot count.

#### `getImportOreFilter(side)`

- Syntax: `getImportOreFilter(side: number): string`
- Purpose: Return the current import bus filter string.
- Parameters:
  - `side: number`
    Multipart side containing the fluid import bus.
- Returns:
  - Current filter string.

#### `setImportOreFilter(side, filter)`

- Syntax: `setImportOreFilter(side: number, filter: string): boolean`
- Purpose: Change the import bus filter string.
- Parameters:
  - `side: number`
    Multipart side containing the fluid import bus.
  - `filter: string`
    New filter string.
- Returns:
  - `true` on success.

### fluid_storagebus

`fluid_storagebus` is exposed on AE2 Fluid Craft fluid storage bus parts.

#### `getStorageConfiguration(side[, slot])`

- Syntax: `getStorageConfiguration(side: number[, slot: number]): table`
- Purpose: Return the configured fluid descriptor in one storage slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid storage bus.
  - `slot: number`
    Optional configuration slot index.
- Returns:
  - A fluid descriptor table.
- Usage notes:
  - If `slot` is omitted, slot `0` is used.
  - Invalid slot indices throw `invalid slot`.

#### `setStorageConfiguration(side[, slot][, detail])`

- Syntax: `setStorageConfiguration(side: number[, slot: number][, detail: table]): boolean`
- Purpose: Set or clear one storage filter slot.
- Parameters:
  - `side: number`
    Multipart side containing the fluid storage bus.
  - `slot: number`
    Optional configuration slot index.
  - `detail: table`
    Optional fluid descriptor table.
- Returns:
  - `true` on success.
- Usage notes:
  - Omitting `detail` clears the selected slot.
  - Invalid slot indices throw `invalid slot`.

#### `getStorageOreFilter(side)`

- Syntax: `getStorageOreFilter(side: number): string`
- Purpose: Return the current storage bus filter string.
- Parameters:
  - `side: number`
    Multipart side containing the fluid storage bus.
- Returns:
  - Current filter string.

#### `setStorageOreFilter(side, filter)`

- Syntax: `setStorageOreFilter(side: number, filter: string): boolean`
- Purpose: Change the storage bus filter string.
- Parameters:
  - `side: number`
    Multipart side containing the fluid storage bus.
  - `filter: string`
    New filter string.
- Returns:
  - `true` on success.

#### `getStorageSlotSize(side)`

- Syntax: `getStorageSlotSize(side: number): number`
- Purpose: Return the number of valid storage configuration slots.
- Parameters:
  - `side: number`
    Multipart side containing the fluid storage bus.
- Returns:
  - Slot count.

### fluid_interface

`fluid_interface` exists both as a block and as a multipart side. The callback names are the same, but the multipart version takes an explicit `side` argument.

#### Block `getFluidInterfaceConfiguration([slot])`

- Syntax: `getFluidInterfaceConfiguration([slot: number]): table`
- Purpose: Return the fluid descriptor configured in one block interface slot.
- Parameters:
  - `slot: number`
    Optional slot index. Defaults to `0`.
- Returns:
  - A fluid descriptor table for that slot.
- Usage notes:
  - If the slot is outside the interface config inventory range, the backing inventory throws an index error.

#### Block `setFluidInterfaceConfiguration([slot][, detail])`

- Syntax: `setFluidInterfaceConfiguration([slot: number][, detail: table]): boolean`
- Purpose: Set or clear one block fluid interface slot.
- Parameters:
  - `slot: number`
    Optional slot index. Defaults to `0`.
  - `detail: table`
    Optional fluid descriptor table.
- Returns:
  - `true` on success.
- Usage notes:
  - Omitting `detail` clears the slot.
  - Unknown fluid ids or names fail immediately instead of being ignored.

#### Multipart `getFluidInterfaceConfiguration(side[, slot])`

- Syntax: `getFluidInterfaceConfiguration(side: number[, slot: number]): table`
- Purpose: Return the fluid descriptor configured in one multipart interface slot.
- Parameters:
  - `side: number`
    Multipart side containing the interface.
  - `slot: number`
    Optional slot index. Defaults to `0`.
- Returns:
  - A fluid descriptor table.
- Usage notes:
  - If the slot is outside the interface config inventory range, the backing inventory throws an index error.

#### Multipart `setFluidInterfaceConfiguration(side[, slot][, detail])`

- Syntax: `setFluidInterfaceConfiguration(side: number[, slot: number][, detail: table]): boolean`
- Purpose: Set or clear one multipart fluid interface slot.
- Parameters:
  - `side: number`
    Multipart side containing the interface.
  - `slot: number`
    Optional slot index. Defaults to `0`.
  - `detail: table`
    Optional fluid descriptor table.
- Returns:
  - `true` on success.
- Usage notes:
  - Omitting `detail` clears the selected slot.
  - `side` must point at an actual `fluid_interface` part or the driver throws `no matching part`.

## Example

```lua
local component = require("component")

if component.isAvailable("fluid_exportbus") then
  local bus = component.fluid_exportbus
  bus.setExportConfiguration(0, 0, {name = "water", amount = 1000, size = 1})
  print("Export slots:", bus.getExportSlotSize(0))
  print("Export filter:", bus.getExportOreFilter(0))
end

if component.isAvailable("fluid_interface") then
  local iface = component.fluid_interface
  iface.setFluidInterfaceConfiguration(0, {name = "lava", amount = 1000})
  local stack = iface.getFluidInterfaceConfiguration(0)
  print(stack and stack.name, stack and stack.amount)
end
```

## Related

- `appeng`
- `thaumicenergistics`
