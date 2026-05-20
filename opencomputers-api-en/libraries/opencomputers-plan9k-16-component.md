# component

## Summary

Plan9k component wrappers that enforce component-group visibility and primary-component tracking.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\16_component.lua`

## Usage

```lua
local component = require("component")
```

## API

### component.get(address, componentType)

- Description: Resolve an abbreviated component address, optionally restricting the search to one component type.
- Parameters:
  - `address`
  - `componentType`

```lua
component.get("address", nil)
```

### component.getPrimary(componentType)

- Description: Return the current primary proxy for one component type, creating it lazily when needed.
- Parameters:
  - `componentType`

```lua
component.getPrimary(nil)
```

### component.isAvailable(componentType)

- Description: Return whether at least one visible component of the requested type is currently available.
- Parameters:
  - `componentType`

```lua
component.isAvailable(nil)
```

### component.isPrimary(address)

- Description: Return whether a component address is the current primary instance for its type.
- Parameters:
  - `address`

```lua
component.isPrimary("address")
```

### component.setPrimary(componentType, address)

- Description: Switch the primary component instance for one type, triggering availability signals when appropriate.
- Parameters:
  - `componentType`
  - `address`

```lua
component.setPrimary(nil, "address")
```

## Notes

- Only exported module functions are listed on this page.
