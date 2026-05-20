# component

## Summary

The `component` runtime API lists visible components, inspects methods, and invokes callbacks by address.

## Availability

- Repository: `opencomputers`
- Type: core runtime API

## Source

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\ComponentAPI.scala`

## Usage

```lua
local component = require("component")
for address, name in component.list() do
  print(address, name)
end
```

## API

### list([filter[, exact]])

- Description: Return visible components, optionally filtered by type.

### type(address)

- Description: Return the component type for the given address.

### slot(address)

- Description: Return the host slot that contains the component.

### methods(address)

- Description: Return callback metadata for a component.

### invoke(address, method, ...)

- Description: Invoke a callback on a remote component.

### doc(address, method)

- Description: Return the source documentation string for a callback.

## Related

- `computer`
- `component`
- `os`
- `unicode`
