# components

## Summary

List visible components and optionally inspect their callable members.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\components.lua`

## Syntax

```lua
components [FILTER...]
```
```lua
components -l [FILTER...]
```
```lua
components --limit=COUNT [FILTER...]
```

## Purpose

Enumerate components returned by `component.list()`. Optional filter arguments restrict the listed component types, `-l` expands each entry with member documentation, and `--limit` stops after a fixed number of matches.

## Parameters

- `FILTER...`: Optional component type filters such as `gpu`, `filesystem`, or `modem`.

## Options

- `-l`: Show component members and the result of `component.doc(address, member)` for each listed component.
- `--limit=COUNT`: Stop after printing at most `COUNT` matching component addresses.

## Examples

```lua
components
```
List every currently visible component address and type.

```lua
components filesystem gpu
```
List only filesystem and GPU components.

```lua
components -l --limit=1 filesystem
```
Show one filesystem component together with its callable members and inline docs.

## Notes

- When multiple filters overlap, components are de-duplicated by address before printing.
- Member inspection relies on `component.proxy`, so it only shows members that are visible through the runtime proxy object.
