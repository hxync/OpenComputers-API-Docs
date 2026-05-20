# mpt

## Summary

Install, remove, and upgrade SecureOS packages with the SecureOS MPT manager.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\mpt.lua`

## Syntax

```lua
mpt -S PACKAGE...
```
```lua
mpt -R PACKAGE...
```
```lua
mpt -u
```
```lua
mpt [OPTIONS]
```

## Purpose

SecureOS ships a variant of `mpt` that keeps its package state under `/var/lib/mpt`, installs named packages with dependencies from the configured MPT backend, removes installed packages, upgrades outdated packages, and can operate against an alternate root directory.

## Parameters

- `PACKAGE...`: One or more package names to install or remove.

## Options

- `-S, --sync`: Install the specified packages and their dependencies.
- `-R, --remove`: Remove the specified installed packages.
- `-u, --upgrades`: Upgrade packages that are older than the versions advertised by the backend.
- `-f, --force`: Force redownload and overwrite behavior during package work.
- `--root=PATH`: Use an alternate root directory instead of `/`.
- `-v`: Enable more verbose logging.
- `-y`: Automatically answer yes to the confirmation prompt.
- `-r, --reboot`: Reboot after the package operation finishes.
- `-h, --help`: Print the built-in help text.

## Examples

```lua
mpt -S opensecurity
```
Install the `opensecurity` package and any missing dependencies.

```lua
mpt -R opensecurity
```
Remove the installed `opensecurity` package.

```lua
mpt -u -y
```
Upgrade outdated packages without stopping for confirmation.

## Notes

- Compared with the Plan9k variant, this SecureOS implementation does not expose package-list update, mirror, or OPPM frontend options.
