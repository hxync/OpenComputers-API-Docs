# flash

## Summary

Read, save, or rewrite the installed EEPROM contents.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\flash.lua`

## Syntax

```lua
flash -l
```
```lua
flash -r FILE
```
```lua
flash [-q] BIOS_FILE [LABEL]
```

## Purpose

Manage the currently installed EEPROM. You can print the current ROM contents, save them to a file, or flash a BIOS file into the EEPROM and optionally update its label.

## Parameters

- `FILE`: Target file used when reading the current EEPROM contents with `-r`.
- `BIOS_FILE`: Binary or Lua BIOS image to write into the EEPROM.
- `LABEL`: Optional new EEPROM label. If omitted in interactive mode, the command prompts for one.

## Options

- `-q`: Quiet mode. Skip confirmation prompts and status messages.
- `-l`: Print the current EEPROM contents to standard output.
- `-r`: Read the current EEPROM contents and save them to a file.

## Examples

```lua
flash -l
```
Print the currently installed EEPROM image.

```lua
flash -r backup.lua
```
Save the current EEPROM contents to `backup.lua`.

```lua
flash bios.lua MyBIOS
```
Write `bios.lua` into the EEPROM and set its label to `MyBIOS`.

## Notes

- Interactive write mode prompts before flashing and warns not to power down during the operation.
- When `-r` is used and the target file already exists, non-quiet mode asks for overwrite confirmation first.
