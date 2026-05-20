# example

## Summary

Example OpenOS `rc` service script that prints a welcome message and tracks how many times it has been started.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `example`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\etc\rc.d\example.lua`

## Notes

- The script defines a `start(msg)` entry point expected by the `rc` service manager.
- Each time it runs, it prints explanatory text, the optional startup message from `/etc/rc.cfg`, the current invocation count, and the current computer runlevel.
