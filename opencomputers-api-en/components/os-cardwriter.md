# os_cardwriter

## Summary

The `os_cardwriter` component is OpenSecurity's card and EEPROM writing station. It can clone blank RFID or magnetic cards into programmed cards, and it can also flash EEPROM items into copies written to output slots.

The block also exposes insert and remove events for the input slot and ships with a bundled filesystem environment named `cardwriter`.

## Availability

- Repository: `opensecurity`
- Component name: `os_cardwriter`
- Typical host: placed OpenSecurity card writer

## Usage

```lua
local component = require("component")
local writer = component.os_cardwriter

if not writer then
  error("os_cardwriter component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Inventory Layout

- Input slot: internal slot `0`
- Output slots used by the callbacks:
  - internal slots `3` through `12`

The callbacks only work when a writable card or EEPROM is present in the input slot and at least one output slot is empty.

## Events

The block emits `computer.signal` events when slot `0` changes:

- `cardInsert`, `"cardInsert"`
- `cardRemove`, `"cardRemove"`

## API

### write

- Syntax: `writer.write(data[, displayName[, locked[, color]]])`
- Returns:
  - Success: `true, uuid`
  - Failure: `false, reason`
- Purpose: Writes data onto a blank RFID or magnetic card and places the written copy into an output slot.

Parameters:

- `data:string`: payload to store on the card.
- `displayName:string` optional: inventory display name for the produced card.
- `locked:boolean` optional: whether the written card should be locked against rewriting.
- `color:number` optional: OpenSecurity color index from `0` to `15`.

Behavior:

- The component consumes `5` buffer energy for each write attempt that reaches the power check.
- Input card requirements:
  - blank RFID card, or
  - blank magnetic card
- Data length limits:
  - RFID: `64` characters
  - magnetic stripe: `128` characters
- Extra characters are silently truncated.
- A UUID is written into the card's tag data if one is not already present.

Color mapping:

- `0` white
- `1` orange
- `2` magenta
- `3` light blue
- `4` yellow
- `5` lime
- `6` pink
- `7` gray
- `8` light gray / silver
- `9` cyan
- `10` purple
- `11` blue
- `12` brown
- `13` green
- `14` red
- `15` black

Common failures:

- `false, "Not enough power in OC Network."`
- `false, "No card in slot"`
- `false, "No Empty Slots"`

Example:

```lua
local ok, uuidOrReason = writer.write("sector-a", "Sector A Access", true, 14)
if not ok then
  error(uuidOrReason)
end
print("Card UUID:", uuidOrReason)
```

### flash

- Syntax: `writer.flash(code, title, locked)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Writes a new EEPROM copy into an empty output slot using an EEPROM item from the input slot.

Parameters:

- `code:string`: EEPROM contents to write as UTF-8 bytes.
- `title:string`: EEPROM label.
- `locked:boolean`: whether the flashed EEPROM should be marked read-only.

Behavior:

- The input item in slot `0` must be an EEPROM.
- A new EEPROM item is created in the first empty output slot.
- The original input EEPROM is consumed by one item.
- Data length:
  - normal configuration: limited to `Settings.get().eepromSize()`
  - bigger EEPROM configuration: limited to `8096` bytes
- Label length:
  - normal configuration: limited to `Settings.get().eepromDataSize()`
  - bigger EEPROM configuration: limited to `512` characters
- Excess data is silently truncated.

Common failures:

- `false, "No EEPROM in slot"`
- `false, "Item is not EEPROM"`
- `false, "No Empty Slots"`
- `false, "Data is Null"`

Example:

```lua
local ok, reason = writer.flash("print('hello')", "Greeter", true)
if not ok then
  error(reason)
end
```

## Notes

- This block also exposes a filesystem environment named `cardwriter` from bundled Lua resources.
- Only blank, unlocked input media are accepted in the input slot.

## Related

- `component`
- `os_biometric`
