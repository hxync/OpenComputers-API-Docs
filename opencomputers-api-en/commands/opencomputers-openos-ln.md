# ln

## Summary

Create a virtual symbolic link.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\ln.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\ln`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\ln`

## Syntax

```lua
ln FILE [TARGET]
```

## Purpose

Create a filesystem link that points at another file or directory path. If no target path is supplied for the new link, the command uses the target's basename in the current working directory.

## Parameters

- `FILE`: Existing file, directory, or symbolic link to reference.
- `TARGET`: Optional path of the new symbolic link.

## Examples

```lua
ln /bin/ls.lua
```
Create `ls.lua` in the current working directory, pointing to `/bin/ls.lua`.

```lua
ln /home/magic.lua /bin/magic.lua
```
Create `/bin/magic.lua` as a link to `/home/magic.lua`.

## Notes

- OpenOS links are virtual entries and do not persist across reboot.
- The command accepts a broken symbolic link as source, but rejects a completely missing source path.
