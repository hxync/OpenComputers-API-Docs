# alias

## Summary

List, inspect, and update shell command aliases.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\alias.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\alias`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\alias`

## Syntax

```lua
alias
```
```lua
alias NAME
```
```lua
alias NAME=value
```

## Purpose

Manage shell aliases that expand one command name into another command line. Without arguments the command prints all aliases, `name` prints one alias, and `name=value` updates it.

## Parameters

- `NAME`: Alias name to query.
- `NAME=value`: Alias assignment expression.

## Options

- `--help`: Print the short usage message.

## Examples

```lua
alias
```
Print all current aliases.

```lua
alias ll
```
Print the current value of alias `ll`.

```lua
alias ll='ls -lh'
```
Set alias `ll` to run `ls -lh`.

```lua
alias ll editor='edit /etc/profile'
```
Query and assign multiple aliases in a single invocation.

## Notes

- Alias names cannot contain shell metacharacters such as `/`, `$`, backticks, `=`, pipes, redirection symbols, or whitespace.
- Values containing spaces should be quoted so the shell passes them as one argument.
