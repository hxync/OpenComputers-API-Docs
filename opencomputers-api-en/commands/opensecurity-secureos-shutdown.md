# shutdown

## Summary

Schedule a SecureOS shutdown or reboot after a delay, or execute it immediately.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\shutdown.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\shutdown`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\shutdown`

## Syntax

```lua
shutdown [-r] [-s] <time|now> [reason]
```

## Purpose

Accept a time value in minutes by default, or in seconds with `-s`, print a status line, sleep for the requested delay, clear the terminal, remove `/tmp/.root`, and finally shut down or reboot the machine.

## Parameters

- `time|now`: Delay before shutdown. `now` means zero delay; otherwise the value is minutes unless `-s` is used.
- `reason`: Optional text appended to the status message shown before the delay starts.

## Options

- `-r`: Reboot instead of powering off.
- `-s`: Interpret the numeric time argument in seconds instead of minutes.

## Examples

```lua
shutdown now maintenance
```
Immediately shut down and announce `maintenance` as the reason.

```lua
shutdown 5 backup
```
Shut down in five minutes and show `backup` as the reason.

```lua
shutdown -r -s 30 deploy
```
Reboot in thirty seconds and announce `deploy` as the reason.

## Notes

- A numeric argument is required unless the literal `now` is used.
- The command removes `/tmp/.root` just before it powers off or reboots, so temporary elevated-root state does not survive the restart.
