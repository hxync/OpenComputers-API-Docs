# irc

## Summary

Connect to an IRC server and chat from a full-screen OpenComputers terminal client.

## Availability

- Repository: `opencomputers`
- System: `opencomputers`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\irc\usr\bin\irc.lua`

## Syntax

```lua
irc NICKNAME [SERVER[:PORT]]
```

## Purpose

Open a TCP connection through the internet card, register the provided nickname, display server traffic in a scrolling full-screen interface, keep the last terminal line for user input, and translate common slash commands into IRC protocol messages.

## Parameters

- `NICKNAME`: Nickname sent in the initial `NICK` and `USER` registration commands.
- `SERVER[:PORT]`: Optional IRC server address. Defaults to `irc.esper.net:6667`.

## Options

- `--motd`: Display the server MOTD lines instead of suppressing them.

## Examples

```lua
irc mynick
```
Connect to the default server with nickname `mynick`.

```lua
irc mynick irc.libera.chat:6667
```
Connect to a specific IRC server and port.

## Notes

- Interactive slash commands implemented in the client include `/msg`, `/join`, `/me`, `/lua`, and raw IRC commands entered as `/COMMAND ...`.
- The client beeps when an incoming message text contains your current nickname.
