# nc

## Summary

Bridge terminal input and output to a TCP socket on the bundled software network stack.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\bin\nc.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\nc`

## Syntax

```lua
nc PORT ADDRESS
```
```lua
nc -l PORT
```

## Purpose

Run in client mode to connect to a remote address and port, or in listen mode to wait for one inbound TCP connection on the chosen port. Incoming TCP payloads are written to the terminal, while released keyboard keys are sent back to the socket one byte at a time.

## Parameters

- `PORT`: TCP port to connect to or listen on.
- `ADDRESS`: Remote network address used in client mode.

## Options

- `-l`: Listen for one inbound connection instead of opening an outbound one.

## Examples

```lua
nc 1234 relay-01
```
Connect to TCP port `1234` on remote address `relay-01`.

```lua
nc -l 1234
```
Listen on local TCP port `1234` until a peer connects.

## Notes

- The program keeps running until interrupted; it does not implement a dedicated disconnect command.
- Input is sent from `key_up` events, so special terminal editing behavior is intentionally minimal.
