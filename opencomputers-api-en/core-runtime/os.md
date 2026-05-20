# os

## Summary

OpenComputers overrides part of Lua's `os` table to expose in-game time semantics instead of host operating system time.

## Availability

- Repository: `opencomputers`
- Type: core runtime API

## Source

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\OSAPI.scala`

## Usage

```lua
print(os.clock())
print(os.date())
```

## API

### clock()

- Description: Return CPU time consumed by the machine.

### date([format[, time]])

- Description: Format an in-game timestamp or return a date table.

### time([table])

- Description: Return the current in-game timestamp or convert a date table.

## Related

- `computer`
- `component`
- `os`
- `unicode`
