# mpt

## Summary

Install, remove, update, and upgrade Plan9k packages from MPT, OPPM, or mirror backends.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\mpt.lua`

## Syntax

```lua
mpt -S PACKAGE...
```
```lua
mpt -R PACKAGE...
```
```lua
mpt -y
```
```lua
mpt -u
```
```lua
mpt [OPTIONS]
```

## Purpose

This is Plan9k's package manager. It keeps its database under `/var/lib/mpt`, can synchronize package lists, install named packages with dependencies, remove installed packages, upgrade outdated packages, work against an alternate root, and optionally use mirror or offline data sources.

## Parameters

- `PACKAGE...`: One or more package names to install or remove.

## Options

- `-S, --sync`: Install the specified packages from a configured remote backend and pull required dependencies.
- `-R, --remove`: Remove the specified installed packages.
- `-y, --update`: Refresh package lists for backends that maintain downloadable indexes.
- `-u, --upgrades`: Upgrade installed packages that are older than the versions offered by active backends.
- `-f, --force`: Force the operation. During upgrades this also forces packages to be re-downloaded.
- `--root=PATH`: Use an alternate filesystem root instead of `/`.
- `-v`: Enable more verbose logging.
- `-Y`: Assume yes for package confirmation prompts.
- `-r, --reboot`: Reboot after package work completes.
- `--offline`: Use only backends marked as offline-capable, such as mirror sources.
- `--mirror=PATH`: Treat another root directory as an offline package mirror source.
- `--oppmRepos=URL`: Override the OPPM repository configuration URL used by the OPPM frontend.
- `-h, --help`: Print the built-in help text.

## Examples

```lua
mpt -Sy
```
Refresh package indexes before doing anything else.

```lua
mpt -S coreutils
```
Install the `coreutils` package together with missing dependencies.

```lua
mpt -R coreutils
```
Remove the installed `coreutils` package.

```lua
mpt -u -Y
```
Upgrade outdated packages without interactive confirmation.

## Notes

- The manager records installed files and metadata inside `/var/lib/mpt`, and it aborts when it detects file conflicts before installation.
- Package removal and upgrade are implemented as file-level operations, so manually editing files installed by a package can lead to conflict or cleanup surprises later.
