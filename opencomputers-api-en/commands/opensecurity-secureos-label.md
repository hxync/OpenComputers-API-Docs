# label

## Summary

Read or change a filesystem label by path or by address.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\label.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\label`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\label`

## Syntax

```lua
label FILE [STRING]
```
```lua
label -a ADDRESS [STRING]
```

## Purpose

Inspect or update the label of a filesystem. You may identify the target either by a path that resolves into the mount, or by its filesystem address when using `-a`.

## Parameters

- `FILE`: Path that resolves to the target filesystem mount.
- `ADDRESS`: Filesystem address or address prefix used with `-a`.
- `STRING`: Optional new label. If omitted, the current label is printed.

## Options

- `-a`: Interpret the first operand as a filesystem address instead of as a path.

## Examples

```lua
label /home
```
Print the label of the filesystem mounted at `/home`.

```lua
label -a 93f test
```
Set the label of the filesystem whose address starts with `93f` to `test`.
