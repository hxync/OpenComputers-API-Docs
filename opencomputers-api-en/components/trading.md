# trading

## Summary

The `trading` component is the top-level interface exposed by the trading upgrade. It discovers nearby merchant offers and returns them as `Trade` userdata objects.

## Availability

- Repository: `opencomputers`
- Lua component name: `trading`
- Typical hosts: robots, drones, and other agents with the trading upgrade installed

## What It Does

- Scans for merchants such as villagers within the configured trading radius.
- Builds one `Trade` userdata object per merchant recipe.
- Assigns a stable per-scan merchant sort id so multiple offers from the same merchant can be grouped together.

## Range Rules

- Only merchants inside the configured OpenComputers trading range are considered.
- The scan area is centered on the host.
- Only entities implementing `IMerchant` are returned.

## API

### getTrades

- Syntax: `trading.getTrades()`
- Returns: `table`
- Purpose: Returns all merchant offers currently in range as `Trade` userdata objects.

The returned table is sorted by merchant identity, then by recipe order for that merchant.

## Returned Trade Objects

Each entry in the returned array is a `Trade` userdata object documented on the companion page:

- [opencomputers-trade.md](opencomputers-trade.md)

## Example

```lua
local component = require("component")
local trading = component.trading

for index, offer in ipairs(trading.getTrades()) do
  local first, second = offer.getInput()
  local output = offer.getOutput()
  print(index, offer.getMerchantId(), output and output.name)
end
```

## Related

- `opencomputers-trade`
- `robot`
- `drone`
