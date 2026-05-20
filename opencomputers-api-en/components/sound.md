# sound

## Summary

The `sound` component is Computronics' programmable sound card. It exposes a real instruction queue, eight channels, several wave generators, volume control, delays, and simple modulation features.

Use it when you need more than short beeps or basic tones. Scripts build a queue of audio instructions and then call `process()` to submit that queue for playback.

## Availability

- Repository: `computronics`
- Component name: `sound`
- Typical host: devices equipped with the Computronics sound card

## Usage

```lua
local component = require("component")
local sound = component.sound

if not sound then
  error("sound component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Core Behavior

- The card provides `8` channels by default.
- Channel numbers in Lua start at `1`.
- Most methods add instructions to an internal build queue instead of taking effect immediately.
- The build queue can hold up to `1024` instructions by default.
- Delays in the queued program are counted in milliseconds, and the total queued delay may not exceed `5000` ms by default.
- You must call `sound.process()` to submit the queued instructions for playback.

Wave modes returned by `sound.modes()`:

- `1 = square`
- `2 = sine`
- `3 = triangle`
- `4 = sawtooth`
- `"noise" = -1` and `-1 = "noise"` for white noise

## API

### channel_count

- Syntax: `sound.channel_count()`
- Returns: `number`
- Purpose: Returns the number of available channels.

Example:

```lua
print("Sound channels:", sound.channel_count())
```

### modes

- Syntax: `sound.modes()`
- Returns: `table`
- Purpose: Returns a bidirectional table of supported waveform modes.

Example:

```lua
local modes = sound.modes()
print("Sine mode id:", modes.sine)
print("Mode 4 name:", modes[4])
print("Noise mode id:", modes.noise)
```

### setTotalVolume

- Syntax: `sound.setTotalVolume(volume)`
- Returns: nothing
- Purpose: Sets the overall output volume of the card immediately.

Parameters:

- `volume:number`: target volume. Values below `0` are clamped to `0`, values above `1` are clamped to `1`.

This is not a queued instruction. It affects the whole card directly.

Example:

```lua
sound.setTotalVolume(0.5)
```

### clear

- Syntax: `sound.clear()`
- Returns: nothing
- Purpose: Clears the current build queue before it is processed.

This does not stop audio that has already been submitted with `process()`. It only removes instructions waiting in the build queue.

Example:

```lua
sound.clear()
```

### open

- Syntax: `sound.open(channel)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues an instruction that opens a channel gate so the channel can generate sound.

Parameters:

- `channel:number`: channel index, starting at `1`.

If the build queue is already full, the call returns `false, "too many instructions"`.

Example:

```lua
assert(sound.open(1))
```

### close

- Syntax: `sound.close(channel)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues an instruction that closes a channel gate and stops further output from that channel.

Parameters:

- `channel:number`: channel index, starting at `1`.

Example:

```lua
assert(sound.close(1))
```

### setWave

- Syntax: `sound.setWave(channel, type)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues a waveform change for a channel.

Parameters:

- `channel:number`: channel index, starting at `1`.
- `type:number`: waveform mode id from `sound.modes()`.

Special values:

- `1` square
- `2` sine
- `3` triangle
- `4` sawtooth
- `-1` white noise

Errors:

- Invalid channel numbers raise an error.
- Invalid mode ids raise `invalid mode: <value>`.

Example:

```lua
sound.setWave(1, sound.modes().sine)
```

### setFrequency

- Syntax: `sound.setFrequency(channel, frequency)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues a frequency change for a channel.

Parameters:

- `channel:number`: channel index, starting at `1`.
- `frequency:number`: frequency in hertz.

The method stores the frequency exactly as given. Unlike the simpler `beep` and `noise` interfaces, this callback does not clamp the frequency in the Lua wrapper.

Example:

```lua
sound.setFrequency(1, 440)
```

### setLFSR

- Syntax: `sound.setLFSR(channel, initial, mask)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues an LFSR noise generator for a channel.

Parameters:

- `channel:number`: channel index, starting at `1`.
- `initial:number`: initial register state.
- `mask:number`: feedback mask.

Use this when you want retro-style pseudo-random noise rather than periodic waveforms.

Example:

```lua
sound.setLFSR(2, 1, 0xB400)
```

### delay

- Syntax: `sound.delay(duration)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues a pause inside the sound program.

Parameters:

- `duration:number`: delay in milliseconds.

Behavior:

- `duration` must be between `0` and `5000` ms by default.
- If the total queued delay would exceed the configured maximum, the call returns `false, "too many delays in queue"`.

Errors:

- Invalid values raise `invalid duration. must be between 0 and 5000`.

Example:

```lua
sound.delay(250)
```

### setFM

- Syntax: `sound.setFM(channel, modIndex, intensity)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues frequency modulation for a channel.

Parameters:

- `channel:number`: channel to be modulated.
- `modIndex:number`: channel acting as the modulator.
- `intensity:number`: modulation strength.

The modulator channel index also uses `1`-based numbering from Lua.

Example:

```lua
sound.setFM(1, 2, 0.5)
```

### resetFM

- Syntax: `sound.resetFM(channel)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues removal of frequency modulation from a channel.

Parameters:

- `channel:number`: target channel.

Example:

```lua
sound.resetFM(1)
```

### setAM

- Syntax: `sound.setAM(channel, modIndex)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues amplitude modulation for a channel.

Parameters:

- `channel:number`: channel to be modulated.
- `modIndex:number`: channel acting as the amplitude modulator.

Example:

```lua
sound.setAM(1, 2)
```

### resetAM

- Syntax: `sound.resetAM(channel)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues removal of amplitude modulation from a channel.

Parameters:

- `channel:number`: target channel.

Example:

```lua
sound.resetAM(1)
```

### setADSR

- Syntax: `sound.setADSR(channel, attack, decay, attenuation, release)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues an ADSR envelope for a channel.

Parameters:

- `channel:number`: target channel.
- `attack:number`: attack duration in milliseconds.
- `decay:number`: decay duration in milliseconds.
- `attenuation:number`: sustain attenuation, normally between `0` and `1`.
- `release:number`: release duration in milliseconds.

The implementation keeps the previous envelope progress if the channel already had one, allowing smoother updates.

Example:

```lua
sound.setADSR(1, 20, 80, 0.6, 120)
```

### resetEnvelope

- Syntax: `sound.resetEnvelope(channel)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues removal of the ADSR envelope from a channel.

Parameters:

- `channel:number`: target channel.

Example:

```lua
sound.resetEnvelope(1)
```

### setVolume

- Syntax: `sound.setVolume(channel, volume)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Queues a per-channel volume change.

Parameters:

- `channel:number`: target channel.
- `volume:number`: desired channel volume.

Behavior:

- The queued volume is clamped internally to the range `0` through `1`.

Example:

```lua
sound.setVolume(1, 0.75)
```

### process

- Syntax: `sound.process()`
- Returns:
  - Success: `true`
  - Failure: `false, reasonOrTime`
- Purpose: Submits the queued instruction program for playback.

Behavior:

- If the queue is empty, it still returns `true`.
- Energy cost depends on the total queued delay time.
- On success, the current build queue is moved into the playback queue.
- If the previous playback queue is still active, the call returns `false, elapsedMilliseconds`, where the second value is how long ago the internal timeout point was crossed.
- If the component network does not have enough energy, the call returns `false, "not enough energy"`.

Example: play a short note.

```lua
sound.clear()
sound.open(1)
sound.setWave(1, sound.modes().square)
sound.setFrequency(1, 440)
sound.setVolume(1, 0.8)
sound.delay(200)
sound.close(1)

local ok, reason = sound.process()
if not ok then
  io.stderr:write("Sound process failed: " .. tostring(reason) .. "\n")
end
```

Example: a two-channel chord.

```lua
sound.clear()
sound.open(1)
sound.open(2)
sound.setWave(1, sound.modes().sine)
sound.setWave(2, sound.modes().sine)
sound.setFrequency(1, 523.25)
sound.setFrequency(2, 659.25)
sound.setVolume(1, 0.6)
sound.setVolume(2, 0.6)
sound.delay(300)
sound.close(1)
sound.close(2)
assert(sound.process())
```

## Notes

- Most callbacks only enqueue instructions. Nothing is heard until `process()` succeeds.
- If you need one-shot tones without queue programming, `beep` or `noise` may be easier to use.

## Related

- `component`
- `beep`
- `noise`
