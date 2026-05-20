# chat_box

## Summary

The block `chat_box` component is a placed chat interface from Computronics. It can post messages to nearby players, listen for nearby player chat, and in some setups also act as a redstone notifier when chat is detected.

Compared with the robot upgrade, the block version has extra behavior for creative-mode variants and global broadcasts.

## Availability

- Repository: `computronics`
- Component name: `chat_box`
- Typical host: placed Computronics chat box block

## Usage

```lua
local component = require("component")
local chatBox = component.chat_box

if not chatBox then
  error("chat_box component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Chat Behavior

- The default configured range is `40` blocks.
- The block listens for nearby player chat and emits a `computer.signal` event:

```lua
"chat_message", username, message
```

- If redstone refresh is enabled in the mod configuration, receiving chat briefly raises the block's redstone output.
- Creative chat boxes behave differently:
  - they may use unlimited or very large configured distance values
  - they can broadcast globally when no explicit distance has been set

## API

### say

- Syntax: `chatBox.say(text[, distance])`
- Returns: `boolean`
- Purpose: Sends a chat message from the block.

Parameters:

- `text:string`: message body.
- `distance:number` optional: temporary radius override for this message.

Behavior:

- If a custom name is set, it is used as the chat prefix.
- Otherwise the default configured prefix is used, usually `ChatBox`.
- If `distance` is omitted:
  - normal chat boxes use the stored distance
  - creative or magic-chat behavior may allow a global send
- If `distance <= 0`, the stored distance is reused.
- The method returns `true` when the string argument was accepted, otherwise `false`.

Example:

```lua
chatBox.setName("BaseConsole")
assert(chatBox.say("Perimeter check complete."))
```

### getDistance

- Syntax: `chatBox.getDistance()`
- Returns: `number`
- Purpose: Returns the stored local chat radius.

Example:

```lua
print("Stored distance:", chatBox.getDistance())
```

### setDistance

- Syntax: `chatBox.setDistance(distance)`
- Returns: `number`
- Purpose: Sets the stored chat radius and returns the effective value.

Parameters:

- `distance:number`: desired radius.

Behavior:

- Values above `32767` are reduced first.
- Normal chat boxes clamp the stored value to the configured chat-box maximum.
- Creative chat boxes may keep larger values.
- Negative values reset the component to the configured default distance and also clear the internal "explicit distance" flag.

Example:

```lua
print("Effective distance:", chatBox.setDistance(24))
```

### getName

- Syntax: `chatBox.getName()`
- Returns: `string`
- Purpose: Returns the currently stored chat prefix name.

Example:

```lua
print("Prefix:", chatBox.getName())
```

### setName

- Syntax: `chatBox.setName(name)`
- Returns: `string`
- Purpose: Sets the custom chat prefix name and returns it.

Parameters:

- `name:string`: new prefix.

Example:

```lua
chatBox.setName("Operations")
```

## Example

Respond to nearby chat messages:

```lua
local event = require("event")

while true do
  local _, _, _, signal, user, message = event.pull("computer.signal")
  if signal == "chat_message" and message == "ping" then
    chatBox.say("pong")
  end
end
```

## Notes

- If the server enables magical chat behavior, the box can listen and send across wider boundaries than a normal local chat radius would suggest.
- The block version persists both its name and its explicit-distance setting.

## Related

- `component`
- `chat`
