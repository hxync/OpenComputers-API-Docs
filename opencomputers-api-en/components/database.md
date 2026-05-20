# database

## Summary

The `database` component stores ghost item stacks for later lookup by other components. It is especially useful when you need stable references to items that differ by damage value or NBT, such as filtered inventory operations, geolyzer block storage, and item comparison logic.

The database does not hold real items. It stores item descriptors in its own slots, and other components can refer to those stored entries by slot or by computed hash.

## Availability

- Repository: `opencomputers`
- Component name: `database`
- Typical host: database upgrade, database block, or other hardware exposing the database component

## Usage

```lua
local component = require("component")
local database = component.database

if not database then
  error("database component not available")
end
```

## API

### get

- Syntax: `database.get(slot)`
- Returns: `table` or `nil`
- Purpose: Reads the stored item representation in a database slot.

Parameter:

- `slot:number`: database slot index.

If the slot is empty, the call returns `nil`.

Example:

```lua
local entry = database.get(1)
if entry then
  print("Stored item:", entry.name, entry.damage)
else
  print("Slot 1 is empty")
end
```

### computeHash

- Syntax: `database.computeHash(slot)`
- Returns: `string` or `nil`
- Purpose: Computes a stable SHA-256 hash for the stored stack in the specified slot.

Parameter:

- `slot:number`: database slot index.

Use this hash together with `indexOf` when you need to locate an identical stored entry later.

If the slot is empty, the call returns `nil`.

Example:

```lua
local hash = database.computeHash(1)
if hash then
  print("Hash:", hash)
end
```

### indexOf

- Syntax: `database.indexOf(hash)`
- Returns: `number`
- Purpose: Finds the first slot whose stored entry matches the supplied hash.

Parameter:

- `hash:string`: hash previously produced by `computeHash`.

Return value details:

- positive slot number when found,
- `-1` when no matching entry exists.

Example:

```lua
local hash = database.computeHash(1)
if hash then
  print("Matching slot:", database.indexOf(hash))
end
```

### clear

- Syntax: `database.clear(slot)`
- Returns: `boolean`
- Purpose: Empties a database slot.

Parameter:

- `slot:number`: database slot index.

Return value details:

- `true` if the slot previously contained an entry,
- `false` if it was already empty.

Example:

```lua
local hadEntry = database.clear(4)
print("Slot 4 had data:", hadEntry)
```

### copy

- Syntax: `database.copy(fromSlot, toSlot[, address])`
- Returns: `boolean`
- Purpose: Copies one stored entry to another slot, optionally into another database.

Parameters:

- `fromSlot:number`: source slot in the current database.
- `toSlot:number`: destination slot.
- `address:string` optional: target database component address. When omitted, copy stays in the same database.

Return value details:

- `true` if the destination slot previously contained an entry,
- `false` if the destination slot was empty before the copy.

Example: copy inside the same database.

```lua
local replaced = database.copy(1, 2)
print("Destination was occupied:", replaced)
```

Example: copy to another database component.

```lua
local other = component.proxy("01234567-89ab-cdef-0123-456789abcdef")
database.copy(1, 1, other.address)
```

### clone

- Syntax: `database.clone(address)`
- Returns: `number`
- Purpose: Copies this database's stored contents into another database component.

Parameter:

- `address:string`: target database address.

Return value details:

- number of slots the source attempted to copy, limited by the smaller database size.

The implementation pauses the computer briefly after cloning, so it is better suited for setup work than hot loops.

Example:

```lua
local copied = database.clone("01234567-89ab-cdef-0123-456789abcdef")
print("Slots copied:", copied)
```

### set

- Syntax: `database.set(slot, id, damage[, nbtJson])`
- Returns: `boolean[, string]`
- Purpose: Stores a ghost stack directly from an item ID, damage value, and optional JSON NBT.

Parameters:

- `slot:number`: destination database slot.
- `id:string`: item registry ID such as `minecraft:wool`.
- `damage:number`: damage or metadata value.
- `nbtJson:string` optional: NBT tag in JSON format. Use `""` or omit it for none.

Return value details:

- `true` on success,
- `false, "invalid item id"` when the item registry ID does not exist.

Example:

```lua
local ok, reason = database.set(1, "minecraft:wool", 14)
if not ok then
  io.stderr:write("Set failed: " .. tostring(reason) .. "\n")
end
```

Example with JSON NBT:

```lua
database.set(2, "minecraft:potion", 0, "{CustomPotionEffects:[{Id:1,Amplifier:1,Duration:200}]}")
```

## Related

- `component`
- `inventory_controller`
- `geolyzer`
