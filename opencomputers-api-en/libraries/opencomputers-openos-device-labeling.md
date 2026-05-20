# device_labeling

## Summary

Load, persist, and apply udev-style label rules for device and filesystem proxies.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\device_labeling.lua`

## Usage

```lua
local device_labeling = require("device_labeling")
```

## API

### device_labeling.getDeviceLabel(proxy)

- Description: Return the explicit label recorded for a proxy, or fall back to the proxy's own `getLabel` method.
- Parameters:
  - `proxy`

```lua
device_labeling.getDeviceLabel(nil)
```

### device_labeling.loadRules(root_dir)

- Description: Read label rules from the configured rules directory and populate the in-memory rule table.
- Parameters:
  - `root_dir`

```lua
device_labeling.loadRules(nil)
```

### device_labeling.saveRule(rule_set, path)

- Description: Write one rule set back to a specific rule file.
- Parameters:
  - `rule_set`
  - `path`

```lua
device_labeling.saveRule(nil, nil)
```

### device_labeling.saveRules(rules)

- Description: Persist all currently loaded rule sets.
- Parameters:
  - `rules`

```lua
device_labeling.saveRules(nil)
```

### device_labeling.setDeviceLabel(proxy, label)

- Description: Assign a label rule for a proxy, or use the proxy's own `setLabel` when supported.
- Parameters:
  - `proxy`
  - `label`

```lua
device_labeling.setDeviceLabel(nil, "value")
```

## Notes

- This helper is usually loaded internally from `/lib/core/device_labeling.lua` rather than imported by end-user programs directly.
