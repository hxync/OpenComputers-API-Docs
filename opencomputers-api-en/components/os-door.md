# os_door

## Summary

The `os_door` component controls an OpenSecurity door controller that is linked to an adjacent security door. It can query the current open state, toggle the door, and manage the password stored inside the linked secure door.

## Availability

- Repository: `opensecurity`
- Component name: `os_door`
- Typical host: placed OpenSecurity door controller

## Usage

```lua
local component = require("component")
local door = component.os_door

if not door then
  error("os_door component not installed")
end
```

You can also access a specific controller with `component.proxy(address)`.

## Controller Requirements

- The controller searches adjacent blocks for an OpenSecurity security door.
- The controller and the linked door must have the same owner UUID.
- Toggling the door consumes `5` buffer energy.
- If the door has a password, open, close, and toggle need that password as the first argument.

## API

### greet

- Syntax: `door.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

Returned text:

```text
Lasciate ogne speranza, voi ch'intrate
```

Example:

```lua
print(door.greet())
```

### isOpen

- Syntax: `door.isOpen()`
- Returns: `boolean`
- Purpose: Returns whether the linked security door is currently open.

Example:

```lua
print("Door open:", door.isOpen())
```

### open

- Syntax: `door.open([password])`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Opens the linked door if it is currently closed.

Behavior:

- If the door is already open, the callback returns `true`.
- If the door is password-protected, the first argument must match the current password.
- Energy cost is paid through the internal `toggle` logic when a state change is required.

Example:

```lua
local ok, reason = door.open("vault123")
if not ok then
  error(reason)
end
```

### close

- Syntax: `door.close([password])`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Closes the linked door if it is currently open.

Behavior:

- If the door is already closed, the callback returns `true`.
- If the door is password-protected, the first argument must match the current password.

Example:

```lua
local ok, reason = door.close("vault123")
if not ok then
  error(reason)
end
```

### toggle

- Syntax: `door.toggle([password])`
- Returns:
  - Success: `boolean`
  - Failure: `false, reason`
- Purpose: Flips the linked security door between open and closed.

Behavior:

- Consumes `5` buffer energy.
- Returns the new open state on success.

Common failures:

- `false, "Password Incorrect"`
- `false, "Owner of Controller and Door do not match."`
- `false, "Not enough power in OC Network."`

Example:

```lua
local openNow, reason = door.toggle("vault123")
if openNow == false and reason then
  error(reason)
end
print("Door state after toggle:", openNow)
```

### setPassword

- Syntax:
  - `door.setPassword(newPassword)`
  - `door.setPassword(oldPassword, newPassword)`
- Returns: `boolean, message`
- Purpose: Sets or changes the password stored in the linked security door.

Behavior:

- If the door currently has no password, pass only the new password.
- If the door already has a password, pass the current password first and the new password second.

Common return values:

- `true, "Password set"`
- `true, "Password Changed"`
- `false, "Password was not changed"`
- `false, "Owner of Controller and Door do not match."`

Example:

```lua
assert(select(1, door.setPassword("vault123")))
```

### removePassword

- Syntax: `door.removePassword(currentPassword)`
- Returns: `boolean, message`
- Purpose: Clears the current password from the linked secure door.

Behavior:

- The supplied password must match the one stored in the door.
- The controller and the door must have the same owner.

Common return values:

- `true, "Password Removed"`
- `false, "Password was not removed"`
- `false, "Owner of Controller and Door do not match."`

Example:

```lua
local ok, message = door.removePassword("vault123")
print(ok, message)
```

## Example

```lua
local password = "vault123"

if not door.isOpen() then
  local ok, reason = door.open(password)
  if not ok then
    error(reason)
  end
end

os.sleep(1)
door.close(password)
```

## Related

- `component`
- `os_biometric`
