# deluser

## Summary

Delete a SecureOS user account and remove its home directory.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\deluser.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\deluser`

## Syntax

```lua
deluser USERNAME
```
```lua
deluser
```

## Purpose

Require root privileges, accept the username from the command line or from an interactive prompt, lowercase it, remove the account via `auth.rmUser`, log the removal, and delete `/home/<username>/` if that directory exists.

## Parameters

- `USERNAME`: Name of the account to remove. The implementation lowercases it first.

## Examples

```lua
deluser steve
```
Remove the account `steve` and delete `/home/steve/` if it exists.

```lua
deluser
```
Open the interactive account-deletion prompt.

## Notes

- The command refuses to run for non-root users.
