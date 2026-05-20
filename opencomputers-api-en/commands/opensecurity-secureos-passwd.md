# passwd

## Summary

Change the password of the currently logged-in SecureOS account.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\passwd.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\passwd`

## Syntax

```lua
passwd
```

## Purpose

Prompt for the current password, then ask for the new password twice. If the current password validates and the two new entries match, SecureOS rewrites the user record through `auth.addUser` while preserving the account's existing superuser flag.

## Examples

```lua
passwd
```
Open the interactive SecureOS password-change prompt for the current account.

## Notes

- If the old password is wrong or the two new passwords differ, the script prints a failure message and leaves the account unchanged.
