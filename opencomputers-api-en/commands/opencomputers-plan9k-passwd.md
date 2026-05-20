# passwd

## Summary

Create or replace the Plan9k login password hash stored in `/etc/passwd`.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\passwd.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\passwd`

## Syntax

```lua
passwd
```

## Purpose

Prompt twice for a new password, generate a random 16-byte HMAC-style key through the data card, hash the password with `component.data.sha256`, and store the packed key-plus-hash blob as Base64 in `/etc/passwd`.

## Examples

```lua
passwd
```

Interactively enter a new Plan9k password and store its salted hash in `/etc/passwd`.

## Notes

- The implementation expects a data card that provides both `random` and `sha256`; without it, password storage cannot succeed reliably.
- The current script does not ask for the old password first.
- The current implementation prints `Passwords do not match` but still continues toward rewriting `/etc/passwd`, so re-run the command immediately if the confirmation entry differed.
