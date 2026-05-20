# os_alarm

## Summary

The `os_alarm` component controls an OpenSecurity alarm block. It can start and stop the alarm, switch to a different installed alarm sound, list available alarm sound files, and optionally play arbitrary sounds at world coordinates when that risky feature is enabled in the mod config.

## Availability

- Repository: `opensecurity`
- Component name: `os_alarm`
- Typical host: placed OpenSecurity alarm block

## Usage

```lua
local component = require("component")
local alarm = component.os_alarm

if not alarm then
  error("os_alarm component not installed")
end
```

You can also access a specific block with `component.proxy(address)`.

## Sound Model

- The alarm stores a current sound id in `soundName`.
- The default sound is `klaxon1`.
- Alarm loudness is stored as a float named `volume`.
- `setRange` does not store the raw block range. It converts the supplied block value into `range / 15 + 0.5`.

## Sound File Notes

`listSounds()` returns the raw filenames found under `mods/OpenSecurity/sounds/alarms/`.

In this version, that list usually contains filenames such as `klaxon1.ogg`, but `setAlarm(name)` checks for `name .. ".ogg"`. In practice:

- `listSounds()` is useful for discovery
- `setAlarm()` usually wants the base name without `.ogg`

## API

### greet

- Syntax: `alarm.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenSecurity greeting.

Returned text:

```text
Lasciate ogne speranza, voi ch'intrate
```

Example:

```lua
print(alarm.greet())
```

### setRange

- Syntax: `alarm.setRange(range)`
- Returns: `string`
- Purpose: Changes the alarm's effective audible range.

Parameters:

- `range:number`: intended block range.

Behavior:

- Valid values are `15` through `150`.
- Successful calls return `"Success"`.
- Invalid values return `"Error, range should be between 15-150"`.

Example:

```lua
print(alarm.setRange(45))
```

### setAlarm

- Syntax: `alarm.setAlarm(name)`
- Returns: `string`
- Purpose: Switches the alarm to another installed alarm sound.

Parameters:

- `name:string`: alarm sound base name, usually without the `.ogg` suffix.

Behavior:

- If `mods/OpenSecurity/sounds/alarms/<name>.ogg` exists, the callback stores that sound and returns `"Success"`.
- Otherwise it returns `"Fail"`.

Example:

```lua
print(alarm.setAlarm("klaxon1"))
```

### activate

- Syntax: `alarm.activate()`
- Returns: `string`
- Purpose: Starts the alarm sound.

Behavior:

- Sets the internal `computerPlaying` flag to `true`.
- Returns `"Ok"`.

Example:

```lua
alarm.activate()
```

### deactivate

- Syntax: `alarm.deactivate()`
- Returns: `string`
- Purpose: Stops the alarm sound.

Behavior:

- Sets the internal `computerPlaying` flag to `false`.
- Returns `"Ok"`.

Example:

```lua
alarm.deactivate()
```

### listSounds

- Syntax: `alarm.listSounds()`
- Returns: `table`
- Purpose: Returns the discovered alarm sound files.

Behavior:

- The table is populated during mod startup by scanning `mods/OpenSecurity/sounds/alarms/`.
- Filenames are returned as stored on disk.

Example:

```lua
for i, name in ipairs(alarm.listSounds()) do
  print(i, name)
end
```

### playSoundAt

- Syntax: `alarm.playSoundAt(x, y, z, sound, range)`
- Returns: `string`
- Purpose: Plays an arbitrary world sound effect at the specified coordinates.

Parameters:

- `x:number`: world X coordinate.
- `y:number`: world Y coordinate.
- `z:number`: world Z coordinate.
- `sound:string`: full Minecraft sound id to play.
- `range:number`: sound radius parameter. The source comment recommends `1` through `10`.

Behavior:

- This callback only works when `playSoundAt` is enabled in the OpenSecurity config.
- Successful calls return `"Ok"`.
- When disabled by config, the callback returns `"Disabled"`.

Example:

```lua
local result = alarm.playSoundAt(0, 64, 0, "minecraft:random.click", 4)
print(result)
```

## Example

```lua
local sounds = alarm.listSounds()
if #sounds > 0 then
  local first = sounds[1]:gsub("%.ogg$", "")
  alarm.setAlarm(first)
end

alarm.setRange(60)
alarm.activate()
os.sleep(2)
alarm.deactivate()
```

## Related

- `component`
- `os_energyturret`
