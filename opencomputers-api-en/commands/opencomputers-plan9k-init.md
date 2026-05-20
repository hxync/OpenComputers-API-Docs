# init

## Summary

Boot the Plan9k user space, services, mounts, and login terminals.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\init.lua`

## Syntax

```lua
init
```

## Purpose

Plan9k runs this script as its first user-space process. It sets up core environment variables, creates `/root`, loads the hostname, starts configured services, opens pseudo-terminals, launches `getty`, `readkey`, and `sh`, mounts filesystems under `/mnt`, and reacts to component hot-plug events.

## Examples

```lua
init
```
Start the full Plan9k user-space session manager.

## Notes

- This script is intended to be the system init process and usually should not be restarted manually from an already running session.
- New filesystem components are mounted automatically below `/mnt/<address-prefix>`, and GPU or screen components trigger new terminal sessions when a matching partner is available.
