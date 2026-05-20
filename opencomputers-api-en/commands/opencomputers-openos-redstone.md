# redstone

## Summary

Inspect and control simple, bundled, or wireless redstone signals.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\redstone.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\redstone`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\de_DE\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\en_US\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\fr_FR\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\ru_RU\block\redstone.md`
- `OpenComputers/src\main\resources\assets\opencomputers\doc\zh_CN\block\redstone.md`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\redstone`

## Syntax

```lua
redstone SIDE [VALUE]
```
```lua
redstone -b SIDE COLOR [VALUE]
```
```lua
redstone -w [VALUE]
```
```lua
redstone -f [FREQUENCY]
```

## Purpose

Use a redstone card or redstone I/O block from the shell. Depending on flags, the command can read or set simple side output, bundled-color output, wireless boolean output, or the active wireless frequency.

## Parameters

- `SIDE`: Side name or numeric side constant such as `front`, `north`, or `3`.
- `VALUE`: Optional output value. Numeric values are used directly; `true`, `on`, and `yes` map to enabled output.
- `COLOR`: Bundled redstone color name or numeric color constant such as `lime`.
- `FREQUENCY`: Wireless redstone frequency number to inspect or set.

## Options

- `-b`: Use bundled redstone mode for the specified side and color.
- `-w`: Read or set the wireless redstone boolean input/output state.
- `-f`: Read or set the active wireless redstone frequency.

## Examples

```lua
redstone front
```
Print the simple redstone input and output values on the front side.

```lua
redstone north 15
```
Set the north-side simple redstone output to strength `15`.

```lua
redstone -b north lime 200
```
Set the bundled `lime` channel on the north side to `200`.

```lua
redstone -w on
```
Enable wireless redstone output and then print current wireless input and output states.

```lua
redstone -f 42
```
Switch the wireless redstone frequency to `42` and print the active frequency.

## Notes

- The command requires a visible `redstone` component.
- Simple outputs use strength `0` to `15`; bundled outputs use `0` to `255`.
- If a value is omitted, the command acts as a read-only status query for the selected mode.
