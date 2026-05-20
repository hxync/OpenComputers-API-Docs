# pty

## Summary

Internal pseudo-terminal endpoints used by Plan9k screen sessions and shell processes.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\18_pty.lua`

## Usage

```lua
-- internal pseudo-terminal object allocated by the Plan9k PTY manager
```

## API

### pty.read(...)

- Description: Read bytes that arrived from the terminal master side into the slave-facing PTY endpoint.
- Parameters:
  - `...`

```lua
pty.read(nil)
```

### pty.write(...)

- Description: Write bytes from the slave-facing PTY endpoint back toward the terminal master side.
- Parameters:
  - `...`

```lua
pty.write(nil)
```

## Notes

- Only exported module functions are listed on this page.
