# rc

## Summary

Inspect, invoke, enable, or disable service commands in `/etc/rc.d/`.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\rc.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rc`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\rc`

## Syntax

```lua
rc SERVICE COMMAND [ARGS...]
```
```lua
rc SERVICE
```

## Purpose

Dispatch a command to a service script stored under `/etc/rc.d/`. If only the service name is given, the command lists the functions exported by that service.

## Parameters

- `SERVICE`: Service script name under `/etc/rc.d/`, without the `.lua` suffix.
- `COMMAND`: Service command such as `start`, `stop`, `restart`, `enable`, or `disable`.
- `ARGS...`: Additional arguments forwarded to the selected service command.

## Examples

```lua
rc example
```
List the commands exported by the `example` service.

```lua
rc example start
```
Start the `example` service.

```lua
rc example enable
```
Enable `example` to be started automatically on boot.

## Notes

- The built-in `enable` and `disable` operations update `/etc/rc.cfg` and do not start or stop the service immediately.
- If a service defines its own command function with the same name, that implementation may override the library default.
