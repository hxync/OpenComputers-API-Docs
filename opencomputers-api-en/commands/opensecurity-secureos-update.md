# update

## Summary

Update SecureOS from the configured release tree or a manually selected tree.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\update.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\update`

## Syntax

```lua
update
```

```lua
update [-a] TREE
```

## Purpose

Require root privileges and an internet card, determine the update tree from the command line or `/etc/update.cfg`, download the remote update script plus the deprecated-package list, remove obsolete files, run the downloaded updater, and finally reboot the machine.

## Parameters

- `TREE`: Remote update tree name. Allowed values: `release` or `dev`.

## Options

- `-a`: Remember the selected tree by writing it to `/etc/update.cfg` for future runs.

## Examples

```lua
update
```

Update using the tree currently stored in `/etc/update.cfg`.

```lua
update dev
```

Pull the development tree once and then reboot into the updated system.

```lua
update -a release
```

Switch the remembered tree to `release`, update from it, and keep using it next time.

## Notes

- The command refuses to continue for non-root users or when no `internet` component is available.
- During the run it downloads temporary files into `/tmp`, including `update-tmp.lua`, `update-tmp.cfg`, and `depreciated.dat`.
- After a successful update it clears `/tmp/.root` and performs `computer.shutdown(true)` to reboot.
