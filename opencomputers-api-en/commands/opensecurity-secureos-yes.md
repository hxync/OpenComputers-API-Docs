# yes

## Summary

Repeat a string forever until the process is interrupted or the output stream closes.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\yes.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\yes`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\yes`

## Syntax

```lua
yes [STRING...]
```
```lua
yes [-V|--version]
```
```lua
yes [-h|--help]
```

## Purpose

Print `y` by default, or print the supplied words joined into one line, then repeat that line continuously. This is commonly used to feed automatic confirmation into programs that ask interactive yes/no questions.

## Parameters

- `STRING...`: Optional words to repeat. If omitted, the program repeats `y`.

## Options

- `-V, --version`: Print the program version banner and exit.
- `-h, --help`: Print the built-in help message and exit.

## Examples

```lua
yes
```
Print `y` on every line until the process is stopped.

```lua
yes no
```
Print `no` on every line.

```lua
yes "I agree" | some-command
```
Continuously feed `I agree` into another program through a pipe.

## Notes

- Stop the loop with `Ctrl-C`, by killing the process, or by closing the downstream output stream.
