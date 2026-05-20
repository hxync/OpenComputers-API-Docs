# wget

## Summary

Download a file over HTTP with an internet card.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\wget.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\wget`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\wget`

## Syntax

```lua
wget URL [FILE]
```

## Purpose

Requests the target URL through the `internet` library and streams the response into a local file. If no target filename is provided, the command derives one from the URL.

## Parameters

- `URL`: HTTP URL to download.
- `FILE`: Optional local filename or path to save the downloaded data.

## Options

- `-f`: Force overwrite if the target file already exists.
- `-q`: Quiet mode. Hide status messages but still print errors.
- `-Q`: Super-quiet mode. Suppress both status output and error messages.

## Examples

```lua
wget http://example.com/data.zip
```
Download `data.zip` and save it using the filename derived from the URL.

```lua
wget -f http://example.com/data.zip /tmp/archive.zip
```
Download the file and overwrite `/tmp/archive.zip` if it already exists.

## Notes

- The command requires an available `internet` component.
- When a download fails after creating the output file, the partial file is removed.
