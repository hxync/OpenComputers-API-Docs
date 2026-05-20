# oppm

## Summary

Bootstrap and, in hidden expert mode, operate the OpenPrograms package manager installer on installation media.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\oppm\usr\bin\oppm.lua`

## Syntax

```lua
oppm
```
```lua
oppm -iKnowWhatIAmDoing list [-i] [FILTER]
```
```lua
oppm -iKnowWhatIAmDoing info PACKAGE
```
```lua
oppm -iKnowWhatIAmDoing install [-f] PACKAGE [PATH]
```
```lua
oppm -iKnowWhatIAmDoing update PACKAGE|all
```
```lua
oppm -iKnowWhatIAmDoing uninstall PACKAGE
```

## Purpose

The bundled `oppm` script is not the normal installed package manager. By default it only prints a warning telling you to run `/bin/install.lua`. If you explicitly pass the hidden `-iKnowWhatIAmDoing` switch, the script can list repositories, show package info, install packages, update packages, and uninstall packages using the bootstrap implementation.

## Parameters

- `FILTER`: Optional substring filter used by `list`.
- `PACKAGE`: Package name to inspect, install, update, or uninstall.
- `PATH`: Optional install destination directory. Defaults to the configured path or `/usr`.

## Options

- `-iKnowWhatIAmDoing`: Unlock the bootstrap package manager commands inside this installer script.
- `-i`: With `list`, show only packages already recorded as installed.
- `-f`: Force directory creation and allow overwriting existing files during installation.
- `--nocache`: Disable repository and package list memoization inside the current run.

## Examples

```lua
oppm
```
Show the installer warning that tells you to run `/bin/install.lua` instead.

```lua
oppm -iKnowWhatIAmDoing list
```
List all visible packages from configured OpenPrograms repositories using the bootstrap client.

```lua
oppm -iKnowWhatIAmDoing install mineos /usr
```
Install the `mineos` package into `/usr` using the bootstrap implementation.

## Notes

- This script stores installed package metadata in `/etc/opdata.svd` and local repository overrides in `/etc/oppm.cfg`.
- Directory-style package declarations that start with `:` are expanded by querying the GitHub contents API before downloading files.
