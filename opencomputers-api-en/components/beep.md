# beep

## Summary

The `beep` component is a simple multi-tone beeper provided by Computronics. It is designed for short alerts, melodies, and status sounds without managing a full instruction queue like the `sound` card.

The card can play up to eight tones at the same time. Each tone is a square wave with a configurable frequency and duration.

## Availability

- Repository: `computronics`
- Component name: `beep`
- Typical host: computers and other OpenComputers devices that can use the Computronics beep card

## Usage

```lua
local component = require("component")
local beep = component.beep

if not beep then
  error("beep component not installed")
end
```

You can also access a specific device with `component.proxy(address)`.

## API

### beep

- Syntax: `beep.beep(frequencyDurationTable)`
- Returns:
  - Success: `true`
  - Failure: `false, reason`
- Purpose: Plays one or more tones immediately.

Parameters:

- `frequencyDurationTable:table`: a Lua table whose keys are frequencies in hertz and whose values are durations in seconds.

Behavior:

- The table may contain at most `8` frequency entries.
- Every frequency must be a number in the range `20` to `2000` Hz.
- Every duration must be numeric when present.
- If a duration is omitted or set to `nil`, the card uses `0.1` seconds.
- Durations are clamped to `50` ms minimum and `5000` ms maximum.
- The card also refuses the call when the new tones plus currently playing tones would exceed `8` simultaneous sounds.

Common failure results:

- `false, "table must not contain more than 8 frequencies"`
- `false, "already too many sounds playing, maximum is 8"`
- `false, reason` when the component network does not have enough energy

Example:

```lua
local ok, reason = beep.beep({
  [440] = 0.15,
  [660] = 0.15,
  [880] = 0.30
})

if not ok then
  io.stderr:write("Beep failed: " .. tostring(reason) .. "\n")
end
```

Example: quick success and error tones.

```lua
local function successTone()
  assert(beep.beep({[880] = 0.08, [1320] = 0.12}))
end

local function errorTone()
  assert(beep.beep({[220] = 0.20, [180] = 0.25}))
end
```

### getBeepCount

- Syntax: `beep.getBeepCount()`
- Returns: `number`
- Purpose: Returns how many beep channels are currently active.

This is useful when you want to avoid sending more tones while the card is still busy.

Example:

```lua
if beep.getBeepCount() == 0 then
  beep.beep({[523.25] = 0.1})
end
```

## Notes

- This component always uses square-wave output.
- Unlike the `sound` card, it does not expose an instruction queue, channel controls, or modulation features.

## Related

- `component`
- `sound`
- `noise`
