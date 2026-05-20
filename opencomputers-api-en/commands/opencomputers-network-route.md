# route

## Summary

Display the MCNET routing table maintained by the bundled software network stack.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\route.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\route`

## Syntax

```lua
route
```

## Purpose

Print every known route returned by `network.info.getRoutes()` in a fixed-width table. The command starts with the banner `MCNET routing table`, then lists each destination together with its gateway router and the interface that will carry the traffic.

## Output

- `Destination`: Target host or network key from the MCNET route table.
- `Gateway`: Router address used to reach that destination.
- `Iface`: Interface through which packets for that destination are sent.

## Examples

```lua
route
```
Display all currently known MCNET route entries.

## Notes

- Column widths are computed dynamically from the longest destination, gateway, and interface values currently present.
- Route order follows Lua `pairs`, so the displayed sequence is not guaranteed to be stable.
