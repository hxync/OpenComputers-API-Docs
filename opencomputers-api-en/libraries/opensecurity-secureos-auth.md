# auth

## Summary

SecureOS password-database helpers for account creation, validation, privilege checks, and audit logging.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\auth.lua`

## Usage

```lua
local auth = require("auth")
```

## API

### auth.addUser(username, password, su)

- Description: Add or replace one password-database entry, hashing the supplied password and storing whether the account is a superuser.
- Parameters:
  - `username`
  - `password`
  - `su`

```lua
auth.addUser("value", nil, nil)
```

### auth.isRoot()

- Description: Return the temporary root marker currently stored in `/tmp/.root`, or `false` when no root marker exists.

```lua
auth.isRoot()
```

### auth.rmUser(username)

- Description: Remove one user entry from the password database and rewrite the passwd file.
- Parameters:
  - `username`

```lua
auth.rmUser("value")
```

### auth.userLog(username, arg)

- Description: Append one authentication audit record to `/var/log/auth.log` when the root filesystem is writable.
- Parameters:
  - `username`
  - `arg`

```lua
auth.userLog("value", nil)
```

### auth.validate(username, password)

- Description: Check whether the supplied password matches the stored hash for one user and return whether that user is also marked as a superuser.
- Parameters:
  - `username`
  - `password`

```lua
auth.validate("value", nil)
```

## Notes

- Only exported module functions are listed on this page.
