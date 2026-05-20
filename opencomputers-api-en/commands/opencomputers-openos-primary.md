# primary

## Summary

Get or change the primary component for a component type.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\primary.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\primary`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\primary`

## Syntax

```lua
primary TYPE
```
```lua
primary TYPE ADDRESS
```

## Purpose

Without an address, the command prints the current primary component address for the requested type. With an address, it switches the primary component and then prints the resulting primary address.

## Parameters

- `TYPE`: Component type, such as `gpu` or `filesystem`.
- `ADDRESS`: Optional full or abbreviated component address to promote to primary.

## Examples

```lua
primary gpu
```
Print the current primary GPU address.

```lua
primary gpu 24a
```
Select the GPU whose address starts with `24a` as the new primary GPU.

## Notes

- Address lookup accepts abbreviated prefixes as long as they resolve uniquely.
- After switching the primary component, the script waits briefly so component signals can settle.
- Note that the address may be abbreviated.
