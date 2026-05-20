# speech_box

## Summary

The `speech_box` component is a placed text-to-speech block from Computronics. It converts text into spoken audio and plays that sound either through adjacent audio receivers or through its own built-in speaker output.

It is useful for talking terminals, voice alarms, announcement systems, and audio-routing setups.

## Availability

- Repository: `computronics`
- Component name: `speech_box`
- Typical host: placed Computronics speech box block

## Usage

```lua
local component = require("component")
local speechBox = component.speech_box

if not speechBox then
  error("speech_box component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Audio Behavior

- The component speaks one phrase at a time.
- If adjacent compatible audio receivers exist, audio is routed into them.
- If no compatible receivers are present, the block uses its own internal speaker.
- Volume is stored as a normalized value from `0` to `1`, internally converted to the audio engine range.

## API

### say

- Syntax: `speechBox.say(text)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Starts speaking the supplied text.

Parameters:

- `text:string`: text to convert to audio.

Behavior:

- If speech is already in progress, the method returns `false, "already processing"`.
- Text length is limited by configuration and defaults to `300` characters.
- If the TTS backend is unavailable, the method returns `false, "text-to-speech system not available"`.

Example:

```lua
local ok, reason = speechBox.say("Welcome to the facility.")
if not ok then
  io.stderr:write("Speech failed: " .. tostring(reason) .. "\n")
end
```

### stop

- Syntax: `speechBox.stop()`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Stops the phrase currently being spoken.

If the box is idle, it returns `false, "not talking"`.

Example:

```lua
speechBox.stop()
```

### isProcessing

- Syntax: `speechBox.isProcessing()`
- Returns: `boolean`
- Purpose: Returns whether the speech box is currently generating or streaming speech audio.

Example:

```lua
while speechBox.isProcessing() do
  os.sleep(0.1)
end
```

### setVolume

- Syntax: `speechBox.setVolume(volume)`
- Returns: no meaningful value
- Purpose: Sets playback volume for future speech.

Parameters:

- `volume:number`: desired volume. Values below `0` clamp to `0`; values above `1` clamp to `1`.

In practice this callback behaves like a setter and should be treated as having no useful return value.

Example:

```lua
speechBox.setVolume(0.65)
```

## Notes

- This component depends on the installed TTS backend.
- Ongoing speech is also stopped automatically when the block unloads or is removed.

## Related

- `component`
- `speech`
