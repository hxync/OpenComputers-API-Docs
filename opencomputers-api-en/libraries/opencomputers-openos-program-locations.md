# programLocations

## Summary

Installer guidance helpers that map program names to craftable floppies and explain missing-command errors.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\tools\programLocations.lua`

## Usage

```lua
local programLocations = dofile("/lib/tools/programLocations.lua")
```

## API

### programLocations.locate(path)

- Description: Look up the floppy or loot source name that can install a program path returned by `computer.getProgramLocations()`.
- Parameters:
  - `path`: Program path or name to look up.

```lua
local loot = programLocations.locate('oppm')
```

### programLocations.reportNotFound(path, reason)

- Description: Print a user-facing not-found message, including install instructions when the missing command is available on a craftable floppy.
- Parameters:
  - `path`: Command path or program name that failed to resolve.
  - `reason`: Optional lower-level resolution error text.

```lua
return programLocations.reportNotFound('oppm', reason)
```

## Notes

- Only exported module functions are listed on this page.
