# tape_rewind

## Summary

This page documents the Computronics tape drive component exposed to OpenComputers. The generated filename is `tape-rewind.md`, but the actual OC component name provided by the block is `tape_drive`.

The component can read and write cassette data, move the tape head, control playback, and adjust playback speed and volume.

## Availability

- Repository: `computronics`
- Actual component name: `tape_drive`
- Typical host: placed Computronics tape drive

## Usage

```lua
local component = require("component")
local tape = component.tape_drive

if not tape then
  error("tape_drive component not installed")
end
```

## Tape Basics

- The drive accepts one cassette tape.
- Tape data can store audio or arbitrary byte data.
- Audio playback uses DFPWM packets internally.
- If no external audio receiver is connected, the drive plays through its internal speaker.

## Playback States

`getState()` returns one of:

- `"STOPPED"`
- `"PLAYING"`
- `"REWINDING"`
- `"FORWARDING"`

Notes:

- The Lua API directly exposes `play()` and `stop()`.
- Fast rewind and fast forward exist internally as states, but the Lua API reaches them only indirectly by changing tape position with `seek()`.

## API

### isEnd

- Syntax: `tape.isEnd()`
- Returns: `boolean`
- Purpose: Returns whether the drive is empty or the tape head has reached the end.

Behavior:

- Also returns `true` when no tape is inserted.

### isReady

- Syntax: `tape.isReady()`
- Returns: `boolean`
- Purpose: Returns whether a tape is inserted.

### getSize

- Syntax: `tape.getSize()`
- Returns: `number`
- Purpose: Returns the tape capacity in bytes.

Behavior:

- Returns `0` if no tape is inserted.

### getPosition

- Syntax: `tape.getPosition()`
- Returns: `number`
- Purpose: Returns the current tape position in bytes.

Behavior:

- Returns `0` if no tape is inserted.

### setLabel

- Syntax: `tape.setLabel(label)`
- Returns: `string|nil`
- Purpose: Sets the cassette label stored in NBT.

Parameters:

- `label:string`: new tape label. An empty string removes the label.

Behavior:

- Returns the stored label after the change.
- Returns `nil` if no tape is inserted.

### getLabel

- Syntax: `tape.getLabel()`
- Returns: `string|nil`
- Purpose: Returns the current cassette label.

### seek

- Syntax: `tape.seek(length)`
- Returns: `number|nil`
- Purpose: Moves the tape head by a signed number of bytes.

Parameters:

- `length:number`: byte offset. Negative values rewind.

Behavior:

- Returns the actual number of bytes moved.
- Returns `nil` if no tape is inserted.
- Movement is clamped by tape bounds.

Example:

```lua
print("Moved:", tape.seek(-1024))
```

### read

- Syntax:
  - `tape.read()`
  - `tape.read(length)`
- Returns:
  - no tape: `nil`
  - without length: `number`
  - with length: `string`
- Purpose: Reads data from the current tape position.

Behavior:

- `tape.read()` reads a single byte and returns it as a number from `0` to `255`.
- `tape.read(length)` reads multiple bytes and returns a Lua string built from those bytes.
- The OpenComputers callback accepts any non-negative integer length.
- The underlying storage will stop at the end of the tape.

Example:

```lua
local byte = tape.read()
local chunk = tape.read(128)
print(byte, #chunk)
```

### write

- Syntax: `tape.write(data)`
- Returns: no meaningful value
- Purpose: Writes one byte or a byte string to the tape.

Parameters:

- `data:number|string`

Behavior:

- A number writes one byte.
- A string writes its raw bytes.
- If no tape is inserted, the callback does nothing.
- Passing the wrong type raises `bad arguments #1 (number or string expected)`.

### play

- Syntax: `tape.play()`
- Returns: `boolean`
- Purpose: Starts audio playback.

Behavior:

- Returns `true` only if a tape is inserted and the state becomes `PLAYING`.

### stop

- Syntax: `tape.stop()`
- Returns: `boolean`
- Purpose: Stops audio playback.

Behavior:

- Returns `true` only if a tape is inserted and the state becomes `STOPPED`.

### setSpeed

- Syntax: `tape.setSpeed(speed)`
- Returns: `boolean`
- Purpose: Sets playback speed.

Parameters:

- `speed:number`: valid range `0.25` through `2.0`

Behavior:

- Internally sets packet size to `round(1024 * speed)`.
- Returns `true` on success, `false` when outside the valid range.

### setVolume

- Syntax: `tape.setVolume(volume)`
- Returns: no meaningful value
- Purpose: Sets playback volume.

Parameters:

- `volume:number`: desired volume

Behavior:

- Values below `0` are clamped to `0`.
- Values above `1` are clamped to `1`.
- Internally this is scaled to `0` through `127`.

### getState

- Syntax: `tape.getState()`
- Returns: `string`
- Purpose: Returns the current drive state string.

## Example

```lua
if not tape.isReady() then
  error("Insert a tape first")
end

print("Size:", tape.getSize())
print("Position:", tape.getPosition())

tape.setLabel("music-a")
tape.setSpeed(1.0)
tape.setVolume(0.5)
tape.seek(0)
tape.play()
os.sleep(1)
tape.stop()
```

## Related

- `component`
- `computronics-computronics-tape-tape`
