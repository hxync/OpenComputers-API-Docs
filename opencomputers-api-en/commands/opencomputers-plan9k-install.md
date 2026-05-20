# install

## Summary

Install files from one filesystem to another, with optional metadata-driven behavior.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\install.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\install`

## Syntax

```lua
install [NAME] [OPTIONS]...
```

## Purpose

Build a list of candidate source and target filesystems, optionally filter them by label or explicit paths, and then copy the source tree into the target tree. When the source root contains `.prop` or `.install`, the installer can set labels, choose boot behavior, request reboot handling, or hand control to a fully custom installation script.

## Parameters

- `NAME`: Optional source label selector, matched case-insensitively against `.prop` metadata.

## Options

- `--from=ADDR`: Select the source filesystem by UUID or by an explicit root path such as `/tmp`.
- `--to=ADDR`: Select the target filesystem by UUID or by an explicit root path.
- `--fromDir=PATH`: Use a relative subdirectory inside the selected source as the install root. Defaults to `.`.
- `--root=PATH`: Use a relative subdirectory inside the target as the destination root.
- `--toDir=PATH`: Alias of `--root=PATH`.
- `-u, --update`: Prompt before modifying files by forwarding interactive/update flags to the underlying copy step.
- `--label=LABEL`: Override the label that would otherwise come from `.prop`.
- `--nosetlabel`: Do not set the target filesystem label.
- `--nosetboot`: Do not set the target filesystem as the default boot device.
- `--noreboot`: Do not reboot automatically after installation completes.

## Examples

```lua
install
```
Prompt for a source filesystem and a target filesystem, then perform the default installation flow.

```lua
install openos
```
Search install sources whose `.prop` label matches `OpenOS` and guide you through choosing a target.

```lua
install --from=/tmp --to=/mnt/disk --root=usr
```
Install from `/tmp` into the `usr` subdirectory of `/mnt/disk`.

## Notes

- A source `.prop` file may define `label`, `setlabel`, `setboot`, `reboot`, and `ignore` fields.
- If a source root contains `.install`, the installer executes that script instead of performing the default recursive copy.
- Custom `.install` scripts receive configuration through `_ENV.install`, including `from`, `to`, `fromDir`, `root`, `update`, `label`, `setlabel`, `setboot`, and `reboot`.
