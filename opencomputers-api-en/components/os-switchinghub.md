# os_switchinghub

## Summary

The `os_switchinghub` component controls an OpenSecurity switchable hub. It acts as a sided OC network relay whose individual faces can be enabled or disabled from Lua.

## Availability

- Repository: `opensecurity`
- Component name: `os_switchinghub`
- Typical host: placed OpenSecurity switchable hub

## Usage

```lua
local component = require("component")
local hub = component.os_switchinghub

if not hub then
  error("os_switchinghub component not installed")
end
```

## Network Behavior

- The block always exposes its primary facing side.
- Additional faces can be individually enabled or disabled.
- After changing a side, the node is removed and rejoined to the OC network so the new topology takes effect.
- Each successful side change consumes `5` buffer energy.

## Password Behavior

- Passwords are optional.
- If no password has been set yet, `setPassword(newPassword)` stores the initial password.
- If a password already exists, `setPassword(oldPassword, newPassword)` changes it only when the first argument matches.
- `setSide` checks the password only when a non-empty password exists.

## Side Argument

`setSide(side, enabled[, password])` passes `side` through Forge direction ids:

- `0`: down
- `1`: up
- `2`: north
- `3`: south
- `4`: west
- `5`: east

The implementation then remaps that local side against the block facing before enabling the physical face.

## API

### greet

- Syntax: `hub.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

### setPassword

- Syntax:
  - `hub.setPassword(newPassword)`
  - `hub.setPassword(oldPassword, newPassword)`
- Returns: `string`
- Purpose: Sets or changes the hub password.

Possible return strings:

- `"Password set"`
- `"Password Changed"`
- `"Password was not changed"`

Example:

```lua
print(hub.setPassword("relay123"))
```

### setSide

- Syntax: `hub.setSide(side, enabled[, password])`
- Returns: `string`
- Purpose: Enables or disables one remapped network face.

Parameters:

- `side:number`: Forge direction id listed above.
- `enabled:boolean`: whether to enable that face.
- `password:string` optional: required only when the hub password is set.

Behavior:

- Returns `"ok"` on success.
- Returns `"Password Incorrect"` if the supplied password does not match.
- Throws `Not enough power in OC Network.` when the network buffer cannot pay the `5` energy cost.

Example:

```lua
local result = hub.setSide(2, true, "relay123")
print(result)
```

## Example

```lua
hub.setPassword("relay123")
hub.setSide(0, true, "relay123")
hub.setSide(1, true, "relay123")
hub.setSide(3, false, "relay123")
```

## Related

- `component`
- `os_door`
