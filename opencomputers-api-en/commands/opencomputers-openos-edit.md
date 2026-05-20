# edit

## Summary

Open a simple full-screen text editor.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\edit.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\edit`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\edit`

## Syntax

```lua
edit FILE
```

## Purpose

Launch the bundled editor on one file. Opening a path that does not yet exist on a writable filesystem prepares a new file that will be created when you save.

## Parameters

- `FILE`: Path of the file to edit or create.

## Options

- `-r`: Open the file in read-only mode.

## Examples

```lua
edit /tmp/test.txt
```
Open `/tmp/test.txt` for editing, creating it on save if needed.

```lua
edit -r /bin/ls.lua
```
Open `/bin/ls.lua` in read-only mode.
