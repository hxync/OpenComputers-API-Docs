# railcraft

## Summary

This page documents the Railcraft integrations exposed to Lua by OpenComputers and Computronics. The available components cover anchors, electric tiles, routing tracks and routing tables, locomotive control tracks, signal boxes, ticket automation, and locomotive relays.

## Availability

- Dependency: `railcraft`
- Label: `integration-required`

## Component Names

- `anchor`
- `boiler_firebox`
- `electric_tile`
- `launcher_track`
- `limiter_track`
- `locomotive_track`
- `powered_track`
- `priming_track`
- `routing_detector`
- `routing_switch`
- `routing_track`
- `digital_controller_box`
- `digital_receiver_box`
- `ticket_machine`
- `locomotive_relay`
- `steam_turbine`

## What This Integration Is Good For

- Monitoring anchor fuel and ownership.
- Reading Railcraft electric charge and per-tick power behavior.
- Automating launcher, limiter, locomotive, priming, powered, and routing tracks.
- Reading and editing routing tables from detector blocks and routing switches.
- Driving Railcraft digital signal networks from Lua.
- Printing and configuring destination tickets.
- Reading and updating the destination of a bound electric locomotive.

## Components

### anchor

`anchor` is exposed by OpenComputers on Railcraft world anchors and related anchor variants.

#### `getFuel()`

- Syntax: `getFuel(): number`
- Purpose: Return the remaining anchor fuel time in ticks.
- Returns:
  - Remaining fuel ticks.
- Usage notes:
  - This is the value Railcraft uses internally for keeping the chunk anchor active.
  - A very low value means the anchor will stop working soon unless more fuel is inserted.

#### `getOwner()`

- Syntax: `getOwner(): string`
- Purpose: Return the anchor owner name.
- Returns:
  - The owner name.
  - No value if the owner is the default internal Railcraft owner.
- Usage notes:
  - Anchors placed without a meaningful player owner may return nothing here.

#### `getType()`

- Syntax: `getType(): string`
- Purpose: Return the anchor type.
- Returns:
  - One of `world`, `admin`, `personal`, `passive`, or `missing`.

#### `getFuelSlotContents()`

- Syntax: `getFuelSlotContents(): table`
- Purpose: Return the item stack currently placed in the anchor fuel slot.
- Returns:
  - An OpenComputers item stack table.
- Usage notes:
  - This is useful when checking what fuel item is loaded without opening the GUI.

#### `isDisabled()`

- Syntax: `isDisabled(): boolean`
- Purpose: Return whether the anchor is disabled by redstone.
- Returns:
  - `true` if redstone power has disabled the anchor.
  - `false` if it is allowed to operate.

### boiler_firebox

`boiler_firebox` is exposed by the Computronics Railcraft bridge on boiler fireboxes.

#### `isBurning()`

- Syntax: `isBurning(): boolean`
- Purpose: Return whether the firebox is currently burning fuel.
- Returns:
  - `true` if the firebox is active.
  - `false` otherwise.

#### `getTemperature()`

- Syntax: `getTemperature(): number`
- Purpose: Return the current firebox temperature.
- Returns:
  - Current temperature value.

#### `getMaxHeat()`

- Syntax: `getMaxHeat(): number`
- Purpose: Return the maximum heat value supported by the firebox.
- Returns:
  - Maximum heat value.

### electric_tile

`electric_tile` is exposed on Railcraft tiles that participate in the Railcraft electric grid.

#### `getCharge()`

- Syntax: `getCharge(): number`
- Purpose: Return the current stored charge.
- Returns:
  - Current charge value.

#### `getCapacity()`

- Syntax: `getCapacity(): number`
- Purpose: Return the maximum charge capacity.
- Returns:
  - Maximum charge the tile can store.

#### `getLoss()`

- Syntax: `getLoss(): number`
- Purpose: Return the charge loss per tick.
- Returns:
  - Per-tick losses reported by the charge handler.

#### `getDraw()`

- Syntax: `getDraw(): number`
- Purpose: Return the charge draw per tick.
- Returns:
  - Per-tick draw value.

### launcher_track

`launcher_track` is exposed on Railcraft launcher tracks.

#### `getForce()`

- Syntax: `getForce(): number`
- Purpose: Return the current launch force configured on the track.
- Returns:
  - The configured force value.

#### `setForce(force)`

- Syntax: `setForce(force: number): boolean[, string]`
- Purpose: Change the launch force used when carts are launched.
- Parameters:
  - `force: number`
    Integer launch force.
- Returns:
  - `true` on success.
  - `false, error` if the value is outside the allowed range.
- Usage notes:
  - Valid values are `5` through the current Railcraft maximum launcher force from config.
  - Invalid values return an error such as `not a valid force value, needs to be between 5 and ...`.

### limiter_track

`limiter_track` is exposed on Railcraft speed limiter tracks.

#### `setLimit(mode)`

- Syntax: `setLimit(mode: number): boolean`
- Purpose: Set the track speed limit mode.
- Parameters:
  - `mode: number`
    Speed limit mode from `1` through `4`.
- Returns:
  - `true` on success.
- Usage notes:
  - OpenComputers rejects invalid values with an argument error.
  - ComputerCraft returns an error similar to `mode needs to be between 1 and 4`.
  - The returned mode values are the Lua-facing values, not the raw NBT byte stored internally.

#### `getLimit()`

- Syntax: `getLimit(): number`
- Purpose: Return the current speed limit mode.
- Returns:
  - A mode value from `1` through `4`.

### locomotive_track

`locomotive_track` is exposed on Railcraft locomotive control tracks.

#### `setMode(mode)`

- Syntax: `setMode(mode: number): boolean`
- Purpose: Set the locomotive mode applied by the track.
- Parameters:
  - `mode: number`
    One of the Lua-facing mode values `0`, `1`, or `2`.
- Returns:
  - `true` on success.
- Usage notes:
  - `0` means `shutdown`.
  - `1` means `idle`.
  - `2` means `running`.
  - Invalid values produce an argument error.

#### `getMode()`

- Syntax: `getMode(): number`
- Purpose: Return the currently configured locomotive mode.
- Returns:
  - `0` for `shutdown`.
  - `1` for `idle`.
  - `2` for `running`.

#### `modes`

- Syntax: `modes: table`
- Purpose: Provide the available locomotive mode names and their Lua-facing numeric values.
- Returns:
  - A table similar to `{ shutdown = 0, idle = 1, running = 2 }`.
- Usage notes:
  - This is exposed as a getter-style value rather than a normal function call in OpenComputers docs.

### powered_track

`powered_track` is exposed on Railcraft powered track variants implementing the powered-track interface.

#### `isPowered()`

- Syntax: `isPowered(): boolean[, string]`
- Purpose: Return whether the track is currently receiving a redstone signal.
- Returns:
  - `true` or `false` when accessible.
  - `nil, "track is locked"` if the track is secure and cannot be queried.

### priming_track

`priming_track` is exposed on Railcraft priming tracks.

#### `getFuse()`

- Syntax: `getFuse(): number`
- Purpose: Return the current fuse time in ticks.
- Returns:
  - Current fuse length.

#### `setFuse(fuse)`

- Syntax: `setFuse(fuse: number): boolean[, string]`
- Purpose: Set the fuse time used by the priming track.
- Parameters:
  - `fuse: number`
    Fuse duration in ticks.
- Returns:
  - `true` on success.
  - `false, error` if the value is outside the valid range.
- Usage notes:
  - Valid values are `0` through `500`.

### routing_detector

`routing_detector` is exposed on Railcraft routing detector blocks that contain a routing table.

#### `getRoutingTable()`

- Syntax: `getRoutingTable(): table[, string]`
- Purpose: Return the full routing table contents as a Lua table of lines.
- Returns:
  - A numeric-keyed table of strings on success.
  - `false, error` if there is no routing table, the table is invalid, or the detector is locked.
- Usage notes:
  - The returned table inserts the marker `"{newpage}"` between pages.
  - The final page marker is omitted.

#### `setRoutingTable(routingTable)`

- Syntax: `setRoutingTable(routingTable: table): boolean[, string]`
- Purpose: Replace the routing table contents.
- Parameters:
  - `routingTable: table`
    Numeric-keyed table of strings.
- Returns:
  - `true` on success.
  - `false, error` on failure.
- Usage notes:
  - Use `"{newline}"` as a page-break marker when writing pages back.
  - The write marker is intentionally `"{newline}"` even though reads return `"{newpage}"`.
  - Failure messages include `no routing table found` and `routing detector is locked`.

#### `getRoutingTableTitle()`

- Syntax: `getRoutingTableTitle(): string[, string]`
- Purpose: Return the title of the routing table item inside the detector.
- Returns:
  - The routing table title.
  - `false, error` if the table is missing or inaccessible.

#### `setRoutingTableTitle(name)`

- Syntax: `setRoutingTableTitle(name: string): boolean[, string]`
- Purpose: Change the title of the routing table item.
- Parameters:
  - `name: string`
    New routing table title.
- Returns:
  - `true` on success.
  - `false, error` on failure.

### routing_switch

`routing_switch` is exposed on Railcraft routing switch motors that hold a routing table.

#### `getRoutingTable()`

- Syntax: `getRoutingTable(): table[, string]`
- Purpose: Return the full routing table currently installed in the switch.
- Returns:
  - A numeric-keyed table of strings.
  - `false, error` if the table is missing, invalid, or the switch is locked.
- Usage notes:
  - Page boundaries are returned as `"{newpage}"`.

#### `setRoutingTable(routingTable)`

- Syntax: `setRoutingTable(routingTable: table): boolean[, string]`
- Purpose: Replace the routing table contents in the switch.
- Parameters:
  - `routingTable: table`
    Numeric-keyed table of strings.
- Returns:
  - `true` on success.
  - `false, error` on failure.
- Usage notes:
  - Use `"{newline}"` to start a new page when writing.
  - A locked switch returns `false, "routing switch is locked"`.

#### `getRoutingTableTitle()`

- Syntax: `getRoutingTableTitle(): string[, string]`
- Purpose: Return the title of the routing table inside the switch.
- Returns:
  - The current title.
  - `false, error` if unavailable.

#### `setRoutingTableTitle(name)`

- Syntax: `setRoutingTableTitle(name: string): boolean[, string]`
- Purpose: Rename the routing table inside the switch.
- Parameters:
  - `name: string`
    New table title.
- Returns:
  - `true` on success.
  - `false, error` on failure.

### routing_track

`routing_track` is exposed on Railcraft routing tracks with a golden ticket inserted.

#### `setDestination(destination)`

- Syntax: `setDestination(destination: string): boolean[, string]`
- Purpose: Write a destination onto the golden ticket installed in the track.
- Parameters:
  - `destination: string`
    Destination name to store.
- Returns:
  - `true` on success.
  - `false, error` if the ticket is missing or the track is locked.
- Usage notes:
  - The track must contain a golden ticket in slot `0`.

#### `getDestination()`

- Syntax: `getDestination(): string[, string]`
- Purpose: Read the destination stored on the golden ticket inside the track.
- Returns:
  - The destination string.
  - `false, error` if the ticket is missing or the track is locked.

### digital_controller_box

`digital_controller_box` is exposed on Computronics Railcraft digital signal controller boxes.

#### `setAspect(name, aspect)`

- Syntax: `setAspect(name: string, aspect: number): boolean[, string]`
- Purpose: Set the aspect for all paired signals with the specified name.
- Parameters:
  - `name: string`
    Paired signal name.
  - `aspect: number`
    Aspect index.
- Returns:
  - `true` on success.
  - `false, "no valid signal found"` if the named signal is not paired.
- Usage notes:
  - Valid aspect values are listed in `aspects`.

#### `setEveryAspect(aspect)`

- Syntax: `setEveryAspect(aspect: number): boolean`
- Purpose: Apply one aspect to every paired signal.
- Parameters:
  - `aspect: number`
    Aspect index from the `aspects` table.
- Returns:
  - `true` on success.

#### `unpair(name)`

- Syntax: `unpair(name: string): boolean[, string]`
- Purpose: Remove all pairings that use the specified signal name.
- Parameters:
  - `name: string`
    Paired signal name.
- Returns:
  - `true` on success.
  - `false, "no valid signal found"` if the name is unknown.

#### `getSignalNames()`

- Syntax: `getSignalNames(): table`
- Purpose: Return every paired receiver name known to this controller.
- Returns:
  - A Lua table of signal names.

#### `aspects`

- Syntax: `aspects: table`
- Purpose: Provide the available Railcraft signal aspects and their numeric indices.
- Returns:
  - A bidirectional table mapping names to indices and indices to names.
- Usage notes:
  - The exported aspects are:
    - `green = 1`
    - `blink_yellow = 2`
    - `yellow = 3`
    - `blink_red = 4`
    - `red = 5`

### digital_receiver_box

`digital_receiver_box` is exposed on Computronics Railcraft digital signal receiver boxes.

#### `getAspect(name)`

- Syntax: `getAspect(name: string): number[, string]`
- Purpose: Return the aspect currently received from the paired signal with the given name.
- Parameters:
  - `name: string`
    Paired signal name.
- Returns:
  - Aspect index on success.
  - `nil, "no valid signal found"` if the name is unknown.

#### `getMostRestrictiveAspect()`

- Syntax: `getMostRestrictiveAspect(): number`
- Purpose: Return the most restrictive aspect currently received from all connected signals.
- Returns:
  - Aspect index.

#### `unpair(name)`

- Syntax: `unpair(name: string): boolean[, string]`
- Purpose: Remove all pairings that use the specified controller name.
- Parameters:
  - `name: string`
    Controller name to unpair.
- Returns:
  - `true` on success.
  - `false, "no valid signal found"` if the name is unknown.

#### `getSignalNames()`

- Syntax: `getSignalNames(): table`
- Purpose: Return every paired controller name known to this receiver.
- Returns:
  - A Lua table of names.

#### `aspects`

- Syntax: `aspects: table`
- Purpose: Provide the available Railcraft signal aspects and their numeric indices.
- Returns:
  - The same bidirectional aspect table used by `digital_controller_box`.

#### Event: `aspect_changed`

- Syntax: `aspect_changed(name, aspectIndex)`
- Purpose: Notify programs when a paired signal changes aspect.
- Parameters:
  - `name: string`
    Signal name that changed.
  - `aspectIndex: number`
    New aspect index.

### ticket_machine

`ticket_machine` is exposed on the Computronics Railcraft ticket machine.

#### `printTicket([amount[, slot]])`

- Syntax: `printTicket([amount: number[, slot: number]]): boolean[, string]`
- Purpose: Queue one or more passenger tickets for printing.
- Parameters:
  - `amount: number`
    Number of tickets to print. Defaults to `1`.
  - `slot: number`
    Ticket slot to use. Defaults to the currently selected slot.
- Returns:
  - `true` on success.
  - `false, error` on common machine failures.
- Usage notes:
  - User-facing ticket slots are `1` through `10`.
  - The machine requires paper in its paper slot.
  - The selected slot must contain a golden ticket that already has a valid destination.
  - Typical failure messages include:
    - `machine is already printing`
    - `no paper found in paper slot`
    - `no golden ticket in specified slot`
    - `tickets not enabled in config`
    - `not enough energy`
    - `output slot already contains ticket with different destination`
    - `output slot is too full`

#### `setManualPrintingAllowed(allowed)`

- Syntax: `setManualPrintingAllowed(allowed: boolean): boolean`
- Purpose: Allow or forbid players from pressing the print button manually.
- Parameters:
  - `allowed: boolean`
    `true` to allow manual printing.
- Returns:
  - The new allowed state.

#### `isManualPrintingAllowed()`

- Syntax: `isManualPrintingAllowed(): boolean`
- Purpose: Return whether manual printing is allowed.
- Returns:
  - `true` if players may print from the GUI.
  - `false` if manual printing is locked.

#### `setManualSelectionAllowed(allowed)`

- Syntax: `setManualSelectionAllowed(allowed: boolean): boolean`
- Purpose: Allow or forbid players from changing the selected ticket slot manually.
- Parameters:
  - `allowed: boolean`
    `true` to allow manual selection.
- Returns:
  - The new allowed state.

#### `isManualSelectionAllowed()`

- Syntax: `isManualSelectionAllowed(): boolean`
- Purpose: Return whether manual ticket selection is allowed.
- Returns:
  - `true` if players may change the selected slot in the GUI.
  - `false` if selection is locked.

#### `setDestination([slot,] destination)`

- Syntax: `setDestination([slot: number,] destination: string): boolean[, string]`
- Purpose: Write a destination onto a golden ticket stored in the machine.
- Parameters:
  - `slot: number`
    Optional ticket slot from `1` through `10`. Defaults to the selected slot.
  - `destination: string`
    Destination name to write.
- Returns:
  - `true` on success.
  - `false, "there is no golden ticket in that slot"` if the slot does not contain a golden ticket.
- Usage notes:
  - Destination length must not exceed `32` characters.

#### `getDestination([slot])`

- Syntax: `getDestination([slot: number]): string[, string]`
- Purpose: Read the destination stored on a golden ticket inside the machine.
- Parameters:
  - `slot: number`
    Optional ticket slot from `1` through `10`. Defaults to the selected slot.
- Returns:
  - The destination string.
  - `false, "there is no golden ticket in that slot"` if the slot is invalid for this operation.

#### `getSelectedTicket()`

- Syntax: `getSelectedTicket(): number`
- Purpose: Return the currently selected ticket slot.
- Returns:
  - Selected slot number, using the user-facing range `1` through `10`.

#### `setSelectedTicket(slot)`

- Syntax: `setSelectedTicket(slot: number): number`
- Purpose: Change the currently selected ticket slot.
- Parameters:
  - `slot: number`
    Slot number from `1` through `10`.
- Returns:
  - The newly selected slot.

### locomotive_relay

`locomotive_relay` is exposed on the Computronics locomotive relay block.

#### `getDestination()`

- Syntax: `getDestination(): string[, string]`
- Purpose: Return the current destination of the bound locomotive.
- Returns:
  - Destination string on success.
  - `nil, error` when the locomotive cannot be accessed.

#### `setDestination(destination)`

- Syntax: `setDestination(destination: string): boolean[, string]`
- Purpose: Change the destination of the bound locomotive.
- Parameters:
  - `destination: string`
    New destination name.
- Returns:
  - `true` on success.
  - `false, "there is no golden ticket inside the locomotive"` if the locomotive has no golden ticket in slot `0`.
  - `nil, error` for relay access failures.

#### `getCharge()`

- Syntax: `getCharge(): number[, string]`
- Purpose: Return the current charge of the bound electric locomotive.
- Returns:
  - Current locomotive charge.
  - `nil, error` if the locomotive cannot be accessed.

#### `getMode()`

- Syntax: `getMode(): string[, string]`
- Purpose: Return the current operating mode of the locomotive.
- Returns:
  - One of `running`, `idle`, or `shutdown`.
  - `nil, error` if the locomotive cannot be accessed.

#### `getName()`

- Syntax: `getName(): string[, string]`
- Purpose: Return the custom display name of the locomotive.
- Returns:
  - The locomotive name.
  - An empty string if the locomotive has no custom name.
  - `nil, error` if the locomotive cannot be accessed.

#### Relay Access Failure Cases

- `relay is not bound to a locomotive`
- `locomotive is currently not detectable`
- `relay and locomotive are in different dimensions`
- `locomotive is too far away`
- `locomotive is locked`
- `locomotive out of energy`
- `not enough energy`

### steam_turbine

`steam_turbine` is exposed by the Computronics Railcraft bridge on Railcraft steam turbines.

#### `getTurbineOutput()`

- Syntax: `getTurbineOutput(): number`
- Purpose: Return the current turbine output value.
- Returns:
  - Current turbine output.

#### `getTurbineRotorStatus()`

- Syntax: `getTurbineRotorStatus(): number`
- Purpose: Return the remaining rotor durability as a percentage.
- Returns:
  - Durability percentage from `0` through `100`.
- Usage notes:
  - Returns `0` if there is no rotor installed in the first inventory slot.

## Example

```lua
local component = require("component")

if component.isAvailable("anchor") then
  local anchor = component.anchor
  print("Anchor type:", anchor.getType())
  print("Fuel:", anchor.getFuel())
end

if component.isAvailable("launcher_track") then
  local launcher = component.launcher_track
  print("Launcher force:", launcher.getForce())
  local ok, err = launcher.setForce(20)
  print("Set force:", ok, err or "")
end

if component.isAvailable("digital_receiver_box") then
  local receiver = component.digital_receiver_box
  local event = {computer.pullSignal()}
  if event[1] == "aspect_changed" then
    print("Signal", event[2], "changed to", event[3])
  end
end
```

## Related

- `buildcraft`
- `computronics`
