# logout

## Summary

Log out the current SecureOS session and return to the login program.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\logout.lua`

## Syntax

```lua
logout
```

## Purpose

Disable autorun if it is still enabled, write a logout audit entry through `auth`, remove the temporary root marker, reset the working directory to `/`, and launch `/boot/z_login.lua`.

## Examples

```lua
logout
```
End the current SecureOS shell session and return to the login screen.

## Notes

- The command removes `/tmp/.root`, so any temporary elevated-root marker stored there is cleared during logout.
