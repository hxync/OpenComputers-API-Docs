# chat

## Summary

The robot `chat` component is a Computronics chat interface that can speak into in-game chat and listen for nearby player chat messages.

It is useful for notification robots, NPC-style interactions, operator consoles, and scripts that react to local chat traffic.

## Availability

- Repository: `computronics`
- Component name: `chat`
- Typical host: robot with the Computronics chat box upgrade installed

## Usage

```lua
local component = require("component")
local chat = component.chat

if not chat then
  error("chat component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Chat Behavior

- The default chat range comes from the Computronics configuration and is `40` blocks by default.
- A custom name is optional. If no custom name is set, the prefix defaults to `ChatBox`.
- Incoming player messages generate a `computer.signal` event named `chat_message`.

Event payload:

```lua
"chat_message", username, message
```

The robot only receives chat messages from players in range unless the server configuration enables the global magic-chat behavior.

## API

### say

- Syntax: `chat.say(text[, distance])`
- Returns: `boolean`
- Purpose: Sends a chat message from the robot using the configured name and range.

Parameters:

- `text:string`: message body to send.
- `distance:number` optional: temporary range override for this message.

Behavior:

- If a custom name is set, the message prefix uses that name.
- Otherwise the prefix falls back to the configured default, usually `ChatBox`.
- If `distance` is supplied, it is clamped to the configured chat-box maximum.
- If `distance <= 0`, the component keeps using the currently stored distance.
- This callback returns `true` when a string message was accepted and queued for delivery.
- It returns `false` when called with invalid argument types.

The robot version never uses a special global-broadcast override from Lua. Even when the modpack enables magic chat behavior, the send helper is still called with a concrete distance.

Example:

```lua
assert(chat.say("Systems online."))
assert(chat.say("Area clear within ten blocks.", 10))
```

### getDistance

- Syntax: `chat.getDistance()`
- Returns: `number`
- Purpose: Returns the currently stored speaking and listening radius.

Example:

```lua
print("Chat radius:", chat.getDistance())
```

### setDistance

- Syntax: `chat.setDistance(distance)`
- Returns: `number`
- Purpose: Sets the stored chat radius and returns the effective new value.

Parameters:

- `distance:number`: requested radius.

Behavior:

- Values above `32767` are cut down before further processing.
- The stored distance is then capped to the configured maximum chat-box distance.
- Negative values reset the distance back to the configured default.

Example:

```lua
local effective = chat.setDistance(16)
print("Using chat distance:", effective)
```

### getName

- Syntax: `chat.getName()`
- Returns: `string`
- Purpose: Returns the custom speaker name currently assigned to the component.

An empty string means the default configured prefix will be used.

Example:

```lua
print("Current chat name:", chat.getName())
```

### setName

- Syntax: `chat.setName(name)`
- Returns: `string`
- Purpose: Sets the custom speaker name and returns the stored value.

Parameters:

- `name:string`: prefix to show inside chat messages.

Example:

```lua
chat.setName("MaintenanceBot")
chat.say("Inspection complete.")
```

## Example

React to nearby player chat and answer automatically:

```lua
local event = require("event")

chat.setName("HelpBot")

while true do
  local _, _, _, signal, user, message = event.pull("computer.signal")
  if signal == "chat_message" and message == "status" then
    chat.say("All monitored systems are nominal.")
  end
end
```

## Related

- `component`
- `speech`
