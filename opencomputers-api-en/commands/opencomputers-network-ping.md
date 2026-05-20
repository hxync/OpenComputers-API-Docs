# ping

## Summary

Send ICMP-like ping probes through the bundled MCNET software network stack.

## Availability

- Repository: `opencomputers`
- System: `network`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\ping.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\ping`

## Syntax

```lua
ping ADDRESS [--count=N] [--size=BYTES] [--interval=SECONDS] [--droptime=SECONDS] [-v]
```

## Purpose

Generate a random payload, send it to the target with `network.icmp.ping`, wait for matching `ping_reply` events, and print round-trip timing plus a final packet-loss summary.

The command supports custom probe count, payload size, interval, drop timeout, and verbose logging.

## Parameters

- `ADDRESS`: Remote MCNET host to ping.

## Options

- `--c=N`, `--count=N`: Number of probes to send.
- `--s=BYTES`, `--size=BYTES`: Payload size in bytes. Default: `56`.
- `--i=SECONDS`, `--interval=SECONDS`: Delay between probes. Default: `1` second.
- `--t=SECONDS`, `--droptime=SECONDS`: Timeout after which a probe is considered lost. Default: `8` seconds.
- `-v`, `--verbose`: Print extra information about transmitted, lost, or malformed packets.
- `-h`, `--help`: Show the built-in usage text.

## Examples

```lua
ping localhost
```

Ping the local machine through the software network stack.

```lua
ping somehost --count=3 --size=128
```

Send three probes with a 128-byte payload to `somehost`.

```lua
ping relay-01 -v --droptime=2
```

Ping `relay-01` with verbose logging and treat unanswered probes as lost after two seconds.

## Notes

- When no explicit count is supplied, the current implementation sends `8` probes.
- Payload bytes are randomized; any generated `:` character is replaced with `_` to avoid confusing the reply matcher.
- The command reports malformed replies separately when the returned payload does not match what was sent.
