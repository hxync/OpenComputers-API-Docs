# sshd

## Summary

Listen for Plan9k SSH connections on TCP port 22 and spawn shell sessions.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sshd.lua`

## Syntax

```lua
sshd
```

## Purpose

Open a TCP listener on port `22`, wait for incoming `network` connection events, and spawn `/usr/sbin/sshsession.lua` for each accepted channel so that the actual authentication and shell relay logic can run in a separate process.

## Examples

```lua
sshd
```
Start the Plan9k SSH listener in the foreground.

## Notes

- The session worker reads `/etc/passwd` and expects the first account entry format used by Plan9k's authentication tooling.
- This listener runs forever until the process is killed.
