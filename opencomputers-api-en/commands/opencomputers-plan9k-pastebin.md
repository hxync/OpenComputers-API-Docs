# pastebin

## Summary

Download from, upload to, or directly run code from Pastebin.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\pastebin.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\pastebin`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\pastebin`

## Syntax

```lua
pastebin get PASTE_ID FILE
```
```lua
pastebin put FILE
```
```lua
pastebin run PASTE_ID [ARGUMENT...]
```

## Purpose

Support three workflows: `get` downloads a paste into a local file, `put` uploads a local file as a new paste, and `run` downloads a paste into a temporary file before executing it through the shell.

## Parameters

- `PASTE_ID`: Pastebin paste identifier used for download or execution.
- `FILE`: Local source file to upload, or destination file to save downloaded data into.
- `ARGUMENT...`: Extra arguments forwarded to the downloaded program when using `run`.

## Options

- `-f`: Allow overwriting an existing destination file during `get`.
- `-k`: Keep downloaded line endings unchanged instead of normalizing Windows CRLF to Unix LF.

## Examples

```lua
pastebin get AbCdEfGh /home/test.lua
```
Download paste `AbCdEfGh` and save it as `/home/test.lua`.

```lua
pastebin put prog.lua
```
Upload `prog.lua` and print the resulting Pastebin URL and paste ID.

```lua
pastebin run AbCdEfGh foo bar
```
Download paste `AbCdEfGh`, execute it, and pass `foo bar` as positional arguments.

## Notes

- An available `internet` component is required for all three subcommands.
- When `run` is used, the downloaded file is stored in a temporary path and removed after execution finishes.
- Uploading consults `/etc/pastebin.conf` for `key`; if it is missing, the bundled default developer key is used.
