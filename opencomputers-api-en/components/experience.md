# experience

## Summary

The `experience` component belongs to the experience upgrade. It stores experience inside the upgrade item itself and exposes a small Lua API for checking the stored level and manually feeding the upgrade with consumable XP sources.

## Availability

- Repository: `opencomputers`
- Lua component name: `experience`
- Typical hosts: robots and drones with an experience upgrade installed

## What It Does

- Stores up to `30` levels of experience in the upgrade.
- Expands the upgrade's internal energy buffer as the stored level rises.
- Keeps the stored experience when the upgrade item is moved to another device.
- Accepts XP bottles and enchanted items as manual experience sources through `consume()`.

The upgrade can also gain experience from gameplay actions handled elsewhere in OpenComputers; this component only exposes the stored amount and manual item consumption.

## API

### level

- Syntax: `experience.level()`
- Returns: `number`
- Purpose: Reads the stored experience as a fractional level value.

The integer part is the current whole level. The decimal part represents progress toward the next level.

### consume

- Syntax: `experience.consume()`
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Consumes one item from the selected inventory slot and converts its experience value into stored upgrade XP.

Accepted items:

- `minecraft:experience_bottle`
- Any enchanted item that yields a positive enchantability value

Failure reasons:

- `max level`
- `no item`
- `could not extract experience from item`
- `could not consume item`

## Practical Notes

- The maximum stored level is `30`.
- XP bottles provide a random amount of experience, matching the vanilla bottle roll used by the implementation.
- Enchanted items are valued from their enchantments; plain items without extractable enchantment value are rejected.
- `consume()` always works on the host's currently selected inventory slot, so scripts must select the slot first if needed.

## Example

```lua
local component = require("component")
local robot = require("robot")
local xp = component.experience

robot.select(1)

local ok, reason = xp.consume()
if ok then
  print(("Stored level: %.2f"):format(xp.level()))
else
  print("Consume failed:", reason)
end
```

## Related

- `robot`
- `drone`
- `generator`
