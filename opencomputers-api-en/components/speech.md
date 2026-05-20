# speech

## Summary

The `speech` component is the Computronics text-to-speech interface. It lets a device speak generated audio through the local audio system instead of manually programming tones.

This component is intended for spoken notifications, accessibility helpers, dialog systems, and voice status readouts.

## Availability

- Repository: `computronics`
- Component name: `speech`
- Typical host: robots or other devices equipped with the Computronics speech upgrade

## Usage

```lua
local component = require("component")
local speech = component.speech

if not speech then
  error("speech component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### say

- Syntax: `speech.say(text)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Starts speaking the supplied text.

Parameters:

- `text:string`: text to convert into spoken audio.

Behavior:

- The device can only process one phrase at a time.
- If speech is already in progress, the call returns `false, "already processing"`.
- The text length must not exceed `300` characters by default.
- If no text-to-speech backend is available, the call returns `false, "text-to-speech system not available"`.

Common failure results:

- `false, "already processing"`
- `false, "text too long"`
- `false, "text-to-speech system not available"`

Example:

```lua
local ok, reason = speech.say("Reactor temperature is stable.")
if not ok then
  io.stderr:write("Speech failed: " .. tostring(reason) .. "\n")
end
```

### stop

- Syntax: `speech.stop()`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Stops the phrase that is currently being spoken.

If the device is idle, it returns `false, "not talking"`.

Example:

```lua
local ok, reason = speech.stop()
if not ok and reason ~= "not talking" then
  io.stderr:write("Stop failed: " .. tostring(reason) .. "\n")
end
```

### isProcessing

- Syntax: `speech.isProcessing()`
- Returns: `boolean`
- Purpose: Returns whether the device is currently speaking or still processing speech data.

Example:

```lua
while speech.isProcessing() do
  os.sleep(0.1)
end
```

### setVolume

- Syntax: `speech.setVolume(volume)`
- Returns: nothing
- Purpose: Sets the playback volume of the speech box.

Parameters:

- `volume:number`: desired volume. Values lower than `0` are clamped to `0`, and values higher than `1` are clamped to `1`.

Example:

```lua
speech.setVolume(0.8)
```

## Notes

- The component depends on the installed text-to-speech backend. If that backend is unavailable, `say` cannot start.
- This interface outputs generated speech audio, not raw phoneme control.

## Related

- `component`
- `sound`
