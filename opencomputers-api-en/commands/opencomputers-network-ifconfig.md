# ifconfig

## Summary

Inspect MCNET interface status and bind extra local addresses.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\ifconfig.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\ifconfig`

## Syntax

```lua
ifconfig
```
```lua
ifconfig bind ADDRESS
```

## Purpose

Without arguments, print a summary for every detected MCNET interface, including its link type, hardware address, received and transmitted packet counts, and byte counters. The only implemented mutating subcommand is `bind`, which forwards the supplied address to `network.ip.bind` and attaches that extra address to the current device.

## Parameters

- `ADDRESS`: Additional local address to attach to this computer when using `bind`.

## Output

- `Link encap`: Link-layer backend name reported by the interface.
- `HWaddr`: Interface hardware or software address.
- `RX packets` / `TX packets`: Packet counters returned by `network.info.getInterfaceInfo`.
- `RX bytes` / `TX bytes`: Byte counters returned by `network.info.getInterfaceInfo`.

## Examples

```lua
ifconfig
```
Print all detected interfaces together with their addressing and traffic counters.

```lua
ifconfig bind relay-01
```
Attach the extra local address `relay-01` to the current node.

## Notes

- The loopback interface `lo` also prints the computer component address as an additional hardware-address line.
- In this build, no validation is performed in the script itself before calling `network.ip.bind`; any rejection comes from the network library.
- Source and bundled manual coverage show no other supported subcommands here.
