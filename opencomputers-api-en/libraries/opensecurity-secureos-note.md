# note

## Summary

Convert between note names, MIDI values, frequencies, note-block ticks, and simple beeps.

## Availability

- Repository: `opensecurity`
- System: `secureos`
- Type: `library`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\note.lua`

## Usage

```lua
local note = require("note")
```

## API

### note.freq(n)

- Description: Convert a note name or MIDI value into a beep frequency.
- Parameters:
  - `n`

```lua
note.freq(nil)
```

### note.midi(n)

- Description: Convert a note name or frequency into a MIDI note number.
- Parameters:
  - `n`

```lua
note.midi(nil)
```

### note.name(n)

- Description: Convert a MIDI note number back into a note name such as `A#4`.
- Parameters:
  - `n`

```lua
note.name(nil)
```

### note.play(tone,duration)

- Description: Play a note immediately through `computer.beep`.
- Parameters:
  - `tone`
  - `duration`

```lua
note.play(nil, nil)
```

### note.ticks(n)

- Description: Convert between note-block tick values `0..24` and their MIDI equivalents `34..58`.
- Parameters:
  - `n`

```lua
note.ticks(nil)
```

## Notes

- Only exported module functions are listed on this page.
