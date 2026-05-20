# iron_noteblock

## Summary

The `iron_noteblock` component is a programmable note emitter from Computronics. It lets Lua queue and play notes using either an explicit instrument or the block's default instrument detection.

It is useful for alarms, melodies, bundled-wire music triggers, and machine feedback sounds.

## Availability

- Repository: `computronics`
- Component name: `iron_noteblock`
- Typical host: placed Computronics iron note block

## Usage

```lua
local component = require("component")
local note = component.iron_noteblock

if not note then
  error("iron_noteblock component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## Instruments

The component accepts either numeric instrument ids or string instrument names.

Supported instrument names:

- `harp`
- `bd`
- `snare`
- `hat`
- `bassattack`
- `pling`
- `bass`

Numeric instruments wrap modulo `7`.

If no instrument is specified, the block chooses a default based on the material below it, similar to note block behavior.

## API

### playNote

- Syntax: `note.playNote([instrument,] note[, volume])`
- Returns: no meaningful value
- Purpose: Queues a note to be played on the next tile update.

Parameters:

- `instrument:number|string|nil` optional:
  - number for a numeric instrument id
  - string for one of the supported instrument names
  - `nil` to force default instrument selection when you still want to pass a note and volume in the three-argument form
- `note:number`: note value. It must be `0` or higher.
- `volume:number` optional: normalized loudness from `0` to `1`.

Behavior:

- Omitted `volume` defaults to `1.0`.
- The Lua-side normalized volume is internally expanded to the block's actual sound scale.
- Invalid volume raises an error such as:
  - `bad argument #N (number between 0 and 1 expected, got X)`
- Invalid instrument names raise:
  - `invalid instrument: name`
- Negative note values raise:
  - `invalid note: value`

Example: default instrument.

```lua
note.playNote(12)
```

Example: explicit named instrument.

```lua
note.playNote("pling", 18, 0.8)
```

Example: numeric instrument id.

```lua
note.playNote(3, 10, 0.5)
```

Example: force default instrument in the three-argument form.

```lua
note.playNote(nil, 7, 1.0)
```

## Notes

- The OpenComputers callback queues the sound task and does not return a useful success flag.
- The block can also react to bundled-wire inputs and play notes directly from bit patterns without Lua.

## Related

- `component`
- `sound`
