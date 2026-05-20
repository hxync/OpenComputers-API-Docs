# refuel

## Summary

Inspect, insert into, or eject from the robot generator upgrade.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\generator\usr\bin\refuel.lua`

## Syntax

```lua
refuel
```
```lua
refuel SLOT [AMOUNT]
```
```lua
refuel all
```

## Purpose

Work with the installed robot generator upgrade. Without arguments the command prints how many items are currently in the generator, a numeric slot refuels from that slot, and `all` tries every inventory slot.

## Parameters

- `SLOT`: Inventory slot number to select before inserting or ejecting items.
- `AMOUNT`: Optional item count. Positive values insert fuel, negative values eject that many items back into the slot, and omitted means up to 64.

## Examples

```lua
refuel
```
Show how many items are currently stored in the generator.

```lua
refuel 3
```
Select slot `3` and insert up to 64 items from that slot into the generator.

```lua
refuel 2 -10
```
Eject ten items from the generator back into slot `2`.

```lua
refuel all
```
Walk through all 16 robot inventory slots and try to insert fuel from each one.

## Notes

- This command intentionally does not use `shell.parse`, so negative amounts such as `-10` are accepted as positional arguments instead of options.
- The generator upgrade must be installed, otherwise the command prints a requirement message and does nothing.
