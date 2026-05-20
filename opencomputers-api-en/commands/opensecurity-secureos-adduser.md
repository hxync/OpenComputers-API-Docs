# adduser

## Summary

Create a SecureOS user account, optionally granting administrator rights.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\adduser.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\adduser`

## Syntax

```lua
adduser USERNAME PASSWORD [true|false]
```
```lua
adduser
```

## Purpose

Require root privileges, then either read the new username and password from the command line or present an interactive prompt. The script lowercases usernames, creates `/home/<username>/` with standard subdirectories, stores the account through `auth.addUser`, and records the action in the user log.

## Parameters

- `USERNAME`: Name of the account to create. The implementation lowercases it before saving.
- `PASSWORD`: Initial password for the new account.
- `true|false`: Optional administrator-rights flag. `true` grants admin rights; omitted defaults to `false`.

## Examples

```lua
adduser steve hunter2 true
```
Create user `steve`, set the password to `hunter2`, and grant administrator rights.

```lua
adduser
```
Open the interactive account-creation prompt.

## Notes

- The command refuses to run for non-root users.
- Interactive mode asks whether to grant root rights using `Y/yes` or `N/no`.
