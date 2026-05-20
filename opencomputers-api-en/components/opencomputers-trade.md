# opencomputers-trade

## Summary

This page documents the `Trade` userdata returned by the `trading` component. Each object represents one specific merchant recipe from one merchant currently in range.

## Availability

- Repository: `opencomputers`
- Runtime type: `Trade` userdata
- Created by: `trading.getTrades()`

## What It Represents

- One merchant
- One recipe index belonging to that merchant
- One live trading opportunity that may later become invalid if the merchant moves away, dies, or disables the recipe

## API

### getMerchantId

- Syntax: `trade.getMerchantId()`
- Returns: `number`
- Purpose: Returns the merchant grouping id assigned by the current `getTrades()` scan.

This id is only a sort/group helper inside the scan result set. It is not a permanent world identifier.

### getInput

- Syntax: `trade.getInput()`
- Returns: `firstStack, secondStack`
- Purpose: Returns the input items the merchant wants for this recipe.

Behavior:

- `firstStack` is always the primary cost.
- `secondStack` is `nil` if the recipe only has one required input.

### getOutput

- Syntax: `trade.getOutput()`
- Returns: `table`
- Purpose: Returns the item stack the merchant offers.

### isEnabled

- Syntax: `trade.isEnabled()`
- Returns: `boolean`
- Purpose: Returns whether the merchant and recipe are still currently usable.

This becomes `false` if the merchant disappears or the recipe becomes disabled.

### trade

- Syntax: `trade.trade()`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Attempts to complete the trade using the host's main inventory.

Possible failure reasons:

- `trading requires an inventory upgrade to be installed`
- `trade has become invalid`
- `out of range`
- `trade is disabled`
- `not enough inventory space to trade`
- `not enough items to trade`

Behavior details:

- The host must have an accessible main inventory.
- The output item must fit before the trade is committed.
- The implementation first tries exact inventory extraction, then retries with looser matching if needed.

## Example

```lua
local component = require("component")
local trading = component.trading

for _, offer in ipairs(trading.getTrades()) do
  if offer.isEnabled() then
    local ok, reason = offer.trade()
    print("Trade result:", ok, reason)
  end
end
```

## Related

- `trading`
- `robot`
- `inventory_controller`
