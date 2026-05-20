# os_rot13

## Summary

Apply ROT13 transformation to standard input or files.

## Availability

- Repository: `opensecurity`
- System: `datablock`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\datablock\bin\os_rot13.lua`

## Syntax

```lua
os_rot13 [FILE...]
```

## Purpose

Read standard input or one or more files completely, apply `datablock.rot13`, and write the transformed text.

## Parameters

- `FILE...`: Optional files to transform. If omitted, read from standard input.

## Examples

```lua
os_rot13 secret.txt
```
Apply ROT13 to `secret.txt` and print the transformed output plus file name.
