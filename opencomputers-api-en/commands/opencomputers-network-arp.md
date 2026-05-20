# arp

## Summary

Inspect the software-network ARP table exposed by the bundled MCNET stack.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\arp.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\arp`

## Syntax

```lua
arp
```

## Purpose

Print the currently known address-resolution entries for each visible network interface. The command gathers interfaces from `network.info.getInfo().interfaces`, reads each interface ARP table with `network.info.getArpTable(interface)`, and formats the result as a simple two-column table.

## Output

- `Address`: Resolved host address known to the local MCNET stack.
- `Iface`: Interface name through which that address was learned.

## Examples

```lua
arp
```
Show the current address-resolution table for all visible MCNET interfaces.

## Notes

- The script always prints the header row `Address` / `Iface`, even when no ARP entries are currently known.
- Entries are grouped by interface in the order returned by Lua `pairs`, so display order is not guaranteed to be stable between runs.
