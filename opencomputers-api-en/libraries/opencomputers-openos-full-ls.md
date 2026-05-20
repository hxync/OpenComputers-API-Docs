# msft

## Summary

Internal Microsoft-style summary helpers used by the extended OpenOS `ls` command.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_ls.lua`

## Usage

```lua
-- internal helpers injected into the extended `ls` implementation
```

## API

### msft.final()

- Description: Print one final grouped filesystem summary after all directory listings have finished.

```lua
msft.final()
```

### msft.report(files, dirs, used, proxy)

- Description: Print one Microsoft-style `File(s)` and `Dir(s)` summary block for a filesystem proxy.
- Parameters:
  - `files`: Number of regular files counted.
  - `dirs`: Number of directories counted.
  - `used`: Total file bytes represented by the listing.
  - `proxy`: Filesystem proxy used to query free and total space.

```lua
msft.report(12, 3, 4096, proxy)
```

### msft.tail(names)

- Description: Accumulate and print summary information for one completed directory listing.
- Parameters:
  - `names`: Collected `ls` entry table for one directory.

```lua
msft.tail(entries)
```

## Notes

- Only exported module functions are listed on this page.
