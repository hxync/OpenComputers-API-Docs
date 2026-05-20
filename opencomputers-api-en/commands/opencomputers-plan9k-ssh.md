# ssh

## Summary

Open an encrypted remote shell session to a Plan9k SSH server on TCP port 22.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\ssh.lua`

## Syntax

```lua
ssh ADDRESS
```

## Purpose

Prompt for a password, connect to the remote address on port `22`, complete the custom challenge-response handshake implemented by Plan9k's `sshsession.lua`, derive a session key, then relay local standard input and remote shell output through encrypted TCP packets.

## Parameters

- `ADDRESS`: Remote host address accepted by `network.tcp.open`.

## Examples

```lua
ssh 192.168.1.10
```
Connect to the remote host at `192.168.1.10` and start an encrypted shell session.

## Notes

- This command depends on Plan9k's `network` stack and the `data` component methods used for random bytes, hashing, encryption, and decryption.
- The remote side must be running the matching Plan9k `sshd` service; this is not OpenSSH compatibility.
