# noise

## Summary

The `noise` component is a lightweight multi-channel sound generator from Computronics. It can either play tones immediately or queue short per-channel note buffers and submit them together.

It exposes eight channels, supports multiple waveform modes, and is easier to drive than the full `sound` card when you only need short notes or effects.

## Availability

- Repository: `computronics`
- Component name: `noise`
- Typical host: devices equipped with the Computronics noise card

## Usage

```lua
local component = require("component")
local noise = component.noise

if not noise then
  error("noise component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### channel_count

- Syntax: `noise.channel_count()`
- Returns: `number`
- Purpose: Returns how many audio channels the card exposes.

The default implementation provides `8` channels.

Example:

```lua
print("Noise channels:", noise.channel_count())
```

### modes

- Syntax: `noise.modes()`
- Returns: `table`
- Purpose: Returns a bidirectional lookup table for valid waveform modes.

The table maps both directions:

- numeric mode id to lowercase mode name
- lowercase mode name to numeric mode id

Built-in waveforms are:

- `1 = square`
- `2 = sine`
- `3 = triangle`
- `4 = sawtooth`

Example:

```lua
local modes = noise.modes()
print("Square mode id:", modes.square)
print("Mode 2 name:", modes[2])
```

### getMode

- Syntax: `noise.getMode(channel)`
- Returns: `number`
- Purpose: Returns the current waveform mode of a channel.

Parameters:

- `channel:number`: channel index, starting at `1`.

Errors:

- Raises an error if the channel is outside `1` to `8`.

Example:

```lua
print("Channel 1 mode:", noise.getMode(1))
```

### setMode

- Syntax: `noise.setMode(channel, mode)`
- Returns: `true`
- Purpose: Changes the waveform mode of a channel.

Parameters:

- `channel:number`: channel index, starting at `1`.
- `mode:number`: waveform id returned by `noise.modes()`.

Errors:

- Raises `"invalid channel"` for an out-of-range channel.
- Raises `"invalid mode"` for an unsupported mode id.

Example:

```lua
local modes = noise.modes()
assert(noise.setMode(1, modes.sine))
```

### add

- Syntax: `noise.add(channel, frequency, duration[, initialDelay])`
- Returns: `true`
- Purpose: Appends one note entry to the specified channel buffer.

Parameters:

- `channel:number`: channel index, starting at `1`.
- `frequency:number`: note frequency in hertz.
- `duration:number`: note duration in seconds.
- `initialDelay:number` optional: delay before this note starts, in seconds.

Behavior:

- Each channel buffer can store at most `8` queued entries.
- `frequency` must be between `20` and `2000` Hz.
- `duration` is clamped to `50` ms minimum and `5000` ms maximum.
- `initialDelay` is clamped to `0` ms minimum and `16000` ms maximum.

Errors and failures:

- Raises `"invalid channel"` for bad channel numbers.
- Raises `"invalid frequency, must be in [20, 2000]"` for bad frequencies.
- Raises `channel N full` when that channel already has 8 buffered entries.

Example:

```lua
noise.setMode(1, noise.modes().square)
noise.add(1, 440, 0.15)
noise.add(1, 660, 0.15, 0.05)
noise.add(1, 880, 0.25, 0.05)
```

### clear

- Syntax: `noise.clear(channel)`
- Returns: nothing
- Purpose: Clears buffered note entries before they are processed.

Behavior:

- The current implementation clears the buffered entries of all channels, even though the callback accepts a `channel` argument.
- Use it as a full buffer reset before rebuilding a pattern.

Example:

```lua
noise.clear(1)
```

### process

- Syntax: `noise.process()`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Sends the currently buffered per-channel sequences for playback and clears the buffers.

Behavior:

- If the card is already processing a previously submitted buffer, it returns `false, "already processing!"`.
- Playback consumes component energy.
- After a successful call, all channel entry buffers are cleared.

Example:

```lua
noise.clear(1)
noise.add(1, 523.25, 0.2)
noise.add(1, 659.25, 0.2)
noise.add(1, 783.99, 0.2)

local ok, reason = noise.process()
if not ok then
  io.stderr:write("Process failed: " .. tostring(reason) .. "\n")
end
```

### getActiveChannels

- Syntax: `noise.getActiveChannels()`
- Returns: `number`
- Purpose: Returns how many playback channels are currently active.

This counts channels that are still busy with sound already sent for playback.

Example:

```lua
print("Channels playing:", noise.getActiveChannels())
```

### isReady

- Syntax: `noise.isReady()`
- Returns: `boolean`
- Purpose: Reports whether the card is ready to accept a new direct playback command.

In practice, this method reflects the card's internal processing state. If it reports `false`, wait a little and try again before calling `play` or `process`.

Example:

```lua
while not noise.isReady() do
  os.sleep(0.05)
end
```

### play

- Syntax: `noise.play(channels)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Immediately plays up to eight channel notes without using the per-channel queued buffer built by `add`.

Parameters:

- `channels:table`: a table indexed by channel number `1` through `8`. Each value is another table where index `1` is frequency in hertz and index `2` is duration in seconds.

Behavior:

- The outer table may contain at most `8` entries.
- Each channel table must be a Lua table.
- Frequency must be between `20` and `2000` Hz.
- Duration defaults to `0.1` seconds when omitted.
- Duration is clamped to `50` ms minimum and `5000` ms maximum.
- A channel that is already marked busy is skipped rather than replaced.

Common failure results:

- `false, "already processing"`
- `false, "table must not contain more than 8 frequencies"`
- `false, reason` when component energy is insufficient

Example:

```lua
local ok, reason = noise.play({
  [1] = {440, 0.20},
  [2] = {660, 0.20},
  [3] = {880, 0.30}
})

if not ok then
  io.stderr:write("Play failed: " .. tostring(reason) .. "\n")
end
```

## Notes

- Channel numbers are `1`-based in Lua.
- The waveform mode set with `setMode` affects both queued playback via `add` and immediate playback via `play`.

## Related

- `component`
- `beep`
- `sound`
