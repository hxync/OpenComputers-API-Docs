# opencomputers-common

## Summary

This filename is misleading: there is no Lua component named `opencomputers-common`.

This page exists to document a legacy mixed export that actually combines callbacks from two real OpenComputers components:

- `configurator`
- `inventory_controller`

## Actual Components Covered Here

### configurator

The `configurator` component is used to inspect and configure adjacent Ender IO conduits.

- Typical hosts: adapter, drone, robot, microcontroller with a configurator-capable environment
- Real Lua component name: `configurator`
- Main purpose: conduit inspection and conduit setting changes

### inventory_controller

Part of this page also contains robot-only upgrade-slot helpers that actually belong to `inventory_controller`.

- Typical host: robot
- Real Lua component name: `inventory_controller`
- Main purpose here: swap tools and installed upgrades

## Configurator Prerequisites

Most `configurator` callbacks on this page require all of the following:

- Ender IO must be installed.
- There must be an Ender IO conduit bundle on the chosen adjacent side.
- For item filter editing, the conduit must already have a compatible filter upgrade installed.

Common failure returns:

- `false, "EnderIO not loaded"`
- `nil, "No conduit here"`
- `nil, "no item conduit here"`
- `nil, "no liquid conduit here"`
- `nil, "no ender conduit here"`

Invalid color or mode strings raise an error instead of returning a clean failure tuple, so pass only supported values.

## Shared Configurator Arguments

The conduit-related callbacks use two side arguments:

- `side:number`: which side of the host contains the adjacent conduit bundle block.
- `conduitDirection:number`: which face of that conduit bundle you want to inspect or configure.

Example:

- On a robot, `side` is interpreted relative to the robot's facing.
- On an adapter, `side` is the world side of the adjacent block.

## Accepted Values

### Dye Colors

Color arguments are case-insensitive after uppercasing, and should use the standard Ender IO dye names:

- `black`
- `red`
- `green`
- `brown`
- `blue`
- `purple`
- `cyan`
- `light_gray`
- `gray`
- `pink`
- `lime`
- `yellow`
- `light_blue`
- `magenta`
- `orange`
- `white`

### Connection Modes

Accepted conduit connection modes:

- `IN_OUT`
- `INPUT`
- `OUTPUT`
- `DISABLED`
- `NOT_SET`

### Extraction Redstone Modes

Accepted extraction redstone modes:

- `IGNORE`
- `ON`
- `OFF`
- `NEVER`

## configurator API

### getConduitConfiguration

- Syntax: `configurator.getConduitConfiguration(side, conduitDirection)`
- Parameters:
  - `side:number`: adjacent conduit bundle position.
  - `conduitDirection:number`: target face on that conduit bundle.
- Returns:
  - Success: `table`
  - Failure: `nil, "No conduit here"` or `nil, "EnderIO not loaded"`
- Purpose: Returns a structured snapshot of the conduit bundle on the chosen side.

Top-level return keys:

- `HasFacade:boolean`
- `ItemConduit:table` when an item conduit exists
- `LiquidConduit:table` when a liquid conduit exists
- `RedstoneConduit:table` when a redstone conduit exists

Possible `ItemConduit` keys:

- `OutputColor`
- `InputColor`
- `RoundRobinEnabled`
- `SelfFeedEnabled`
- `ExtractionRedstoneConditionMet`
- `OutputPriority`
- `InputFilter`
- `OutputFilter`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

Possible `LiquidConduit` keys on Ender Fluid Conduits:

- `InputFilter`
- `OutputFilter`
- `InputColor`
- `OutputColor`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

Possible `LiquidConduit` keys on normal tank conduits:

- `FluidType`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

Possible `RedstoneConduit` keys:

- `SignalColor`
- `IsOutputStrong`
- `ConnectionMode`

Item filter tables may contain:

- `Advanced`
- `Blacklist`
- `MatchMeta`
- `MatchNBT`
- `UseOreDict`
- `Sticky`
- `FuzzyMode`
- `FilterItems`

Fluid filter tables may contain:

- `blacklist`
- `"1"` through `"5"` for the configured fluid names in each filter slot

### setItemConduitColor

- Syntax: `configurator.setItemConduitColor(side, conduitDirection, isInput, color)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`: `true` for input color, `false` for output color.
  - `color:string`: one of the accepted dye colors listed above.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Sets the input or output color of an item conduit connection.

### setItemConduitOutputPriority

- Syntax: `configurator.setItemConduitOutputPriority(side, conduitDirection, priority)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `priority:number`: numeric priority applied to that output face.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Sets the output priority of an item conduit face.

### setItemConduitFilter

- Syntax: `configurator.setItemConduitFilter(side, conduitDirection, databaseAddress, dbSlot, filterIndex, isInput)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `databaseAddress:string`: address of a database component on the same network.
  - `dbSlot:number`: database slot to copy from.
  - `filterIndex:number`: target slot inside the conduit filter. This index is passed through directly and is effectively `0`-based.
  - `isInput:boolean`: `true` for input filter, `false` for output filter.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `false, "Wrong or no item filter"` or conduit/database related errors
- Purpose: Copies one item stack from a database entry into an item conduit filter slot.

Usage notes:

- The conduit must already have a normal item filter that can accept direct slot edits.
- `dbSlot` follows normal OpenComputers slot validation.

### setItemConduitConnectionMode

- Syntax: `configurator.setItemConduitConnectionMode(side, conduitDirection, mode)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`: one of `IN_OUT`, `INPUT`, `OUTPUT`, `DISABLED`, `NOT_SET`.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Sets the item conduit connection mode for that face.

### setItemConduitExtractionRedstoneColor

- Syntax: `configurator.setItemConduitExtractionRedstoneColor(side, conduitDirection, color)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `color:string`: one of the accepted dye colors.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Sets the extraction redstone color used by that item conduit face.

### setItemConduitExtractionRedstoneMode

- Syntax: `configurator.setItemConduitExtractionRedstoneMode(side, conduitDirection, mode)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`: one of `IGNORE`, `ON`, `OFF`, `NEVER`.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Sets when redstone allows that item conduit face to extract.

### setEnderLiquidConduitFilter

- Syntax: `configurator.setEnderLiquidConduitFilter(side, conduitDirection, fluid, blacklist, isInput[, filterIndex])`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `fluid:string`: registered fluid name, such as `water` or `lava`.
  - `blacklist:boolean`: `true` to blacklist the fluid, `false` to whitelist it.
  - `isInput:boolean`: `true` to change the input filter, `false` for output.
  - `filterIndex:number`: optional filter slot index, default `0`. Ender fluid filters expose five slots, indexed `0` to `4`.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no ender conduit here"`
- Purpose: Replaces one filter entry in an Ender Fluid Conduit.

### setEnderLiquidConduitColor

- Syntax: `configurator.setEnderLiquidConduitColor(side, conduitDirection, isInput, color)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`
  - `color:string`
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no ender conduit here"`
- Purpose: Sets the input or output color of an Ender Fluid Conduit face.

### setLiquidConduitConnectionMode

- Syntax: `configurator.setLiquidConduitConnectionMode(side, conduitDirection, mode)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`: one of `IN_OUT`, `INPUT`, `OUTPUT`, `DISABLED`, `NOT_SET`.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no liquid conduit here"`
- Purpose: Sets the connection mode of a liquid conduit face.

### setLiquidConduitExtractionRedstoneColor

- Syntax: `configurator.setLiquidConduitExtractionRedstoneColor(side, conduitDirection, color)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `color:string`
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no liquid conduit here"`
- Purpose: Sets the extraction redstone color of a liquid conduit face.

### setLiquidConduitExtractionRedstoneMode

- Syntax: `configurator.setLiquidConduitExtractionRedstoneMode(side, conduitDirection, mode)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`: one of `IGNORE`, `ON`, `OFF`, `NEVER`.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no liquid conduit here"`
- Purpose: Sets the extraction redstone mode of a liquid conduit face.

### setRedstoneConduitSignalColor

- Syntax: `configurator.setRedstoneConduitSignalColor(side, conduitDirection, color)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `color:string`
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no liquid conduit here"`
- Purpose: Sets the color channel of an insulated redstone conduit connection.

### setRedstoneConduitSignalStrength

- Syntax: `configurator.setRedstoneConduitSignalStrength(side, conduitDirection, strength)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `strength:boolean`: `true` for strong output, `false` for weak output.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no liquid conduit here"`
- Purpose: Sets whether an insulated redstone conduit outputs strong or weak redstone power.

### replaceConduitFilter

- Syntax: `configurator.replaceConduitFilter(side, conduitDirection, isInput)`
- Parameters:
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`: `true` for input filter, `false` for output filter.
- Returns:
  - Success: `true`
  - Failure: `false, "EnderIO not loaded"` or `nil, "no item conduit here"`
- Purpose: Robot-only helper that swaps the conduit filter upgrade with the item stack in the robot's currently selected slot.

Behavior details:

- Only the robot variant of `configurator` exposes this callback.
- The old conduit filter item is moved into the robot's selected slot.

## inventory_controller API On Robots

These callbacks belong to the real `inventory_controller` component, not to `configurator`.

### equip

- Syntax: `inventoryController.equip()`
- Returns: `boolean`
- Purpose: Swaps the robot's equipped tool slot with the robot's currently selected internal inventory slot.

### installUpgrade

- Syntax: `inventoryController.installUpgrade([slot])`
- Parameters:
  - `slot:number`: optional robot container slot index, default `1`.
- Returns:
  - Success: `true`
  - Failure: `false`, `false, "not a container slot"`, or `false, "Invalid upgrade"`
- Purpose: Swaps the upgrade installed in the chosen robot container slot with the item in the currently selected robot inventory slot.

### getUpgradeContainerType

- Syntax: `inventoryController.getUpgradeContainerType(slot)`
- Parameters:
  - `slot:number`: robot container slot index.
- Returns: `string`
- Purpose: Returns the container type accepted by that robot slot.

Behavior details:

- If the slot is not a container slot, the function returns `"None"`.

### getUpgradeContainerTier

- Syntax: `inventoryController.getUpgradeContainerTier(slot)`
- Parameters:
  - `slot:number`: robot container slot index.
- Returns: `number`
- Purpose: Returns the tier requirement of that robot container slot.

Behavior details:

- If the slot is not a container slot, the function returns `0`.

## Examples

### Example: inspect an adjacent conduit

```lua
local component = require("component")
local sides = require("sides")

local configurator = component.configurator
local info = configurator.getConduitConfiguration(sides.front, sides.up)

print("Has facade:", info.HasFacade)
if info.ItemConduit then
  print("Item mode:", tostring(info.ItemConduit.ConnectionMode))
  print("Output priority:", info.ItemConduit.OutputPriority)
end
```

### Example: install a robot upgrade

```lua
local component = require("component")
local robot = require("robot")

local inventoryController = component.inventory_controller

robot.select(1)
local ok, err = inventoryController.installUpgrade(1)
print("Upgrade installed:", ok, err)
```

## Related

- `opencomputers-inventory-control`
- `robot`
- `adapter`
