# dmesg

## Summary

Continuously print incoming events until interrupted.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\dmesg.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\dmesg`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\dmesg`

## Syntax

```lua
dmesg [EVENT...]
```

## Purpose

Listen for all events, or only the named events, and print each event tuple with a timestamp. The command keeps running until an `interrupted` signal is received, usually by pressing `Ctrl-C`.

## Parameters

- `EVENT...`: Optional event names to filter, such as `touch`, `key_down`, or `modem_message`.

## Examples

```lua
dmesg
```
Print every incoming event until the session is interrupted.

```lua
dmesg touch key_down
```
Only show `touch`, `key_down`, and the mandatory `interrupted` event.

## Notes

- On interactive terminals the event name, address, and payload columns are colorized.
- Even when filters are supplied, the command always listens for `interrupted` so it can exit cleanly.
