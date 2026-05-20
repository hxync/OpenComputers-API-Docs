# sh

## Summary

Advanced shell completion and file/program lookup helpers layered onto the base `sh` module.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_sh.lua`

## Usage

```lua
local sh = require("sh")  -- advanced parsing helpers load lazily
```

## API

### sh.getMatchingFiles(partial_path)

- Description: Return filesystem completion candidates for one partially typed path.
- Parameters:
  - `partial_path`: Partial path exactly as typed by the user, possibly relative and possibly escaped.

```lua
local files = sh.getMatchingFiles('/et')
```

### sh.getMatchingPrograms(baseName)

- Description: Return command-name completion candidates from shell aliases and every directory in `PATH`.
- Parameters:
  - `baseName`: Prefix typed by the user.

```lua
local programs = sh.getMatchingPrograms('gr')
```

## Notes

- Only exported module functions are listed on this page.
