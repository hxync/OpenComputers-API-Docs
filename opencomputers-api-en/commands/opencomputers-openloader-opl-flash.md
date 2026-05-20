# opl-flash

## Summary

Flash the bundled OpenLoader bootloader into the installed EEPROM.

## Availability

- Repository: `opencomputers`
- System: `openloader`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openloader\bin\opl-flash.lua`

## Syntax

```lua
opl-flash [-hqr] [--label=EEPROM_LABEL]
```

## Purpose

Write the bundled OpenLoader EEPROM image into the current EEPROM component and optionally assign a label or reboot immediately afterward.

## Parameters

- `EEPROM_LABEL`: Optional label to assign after flashing. If omitted, the command uses the built-in OpenLoader version string.

## Options

- `-q, --quiet`: Do not print prompts or status output, and do not ask for confirmation.
- `-r, --reboot`: Reboot immediately after flashing succeeds.
- `--label=EEPROM_LABEL`: Set the EEPROM label after flashing.
- `-h, --help`: Print the built-in help text.

## Examples

```lua
opl-flash
```
Prompt for confirmation and flash OpenLoader using the default version label.

```lua
opl-flash --label=GTNH -r
```
Flash OpenLoader, label the EEPROM as `GTNH`, and reboot immediately.

## Notes

- Quiet mode implies non-interactive confirmation; the flash proceeds immediately.
