# self_destruct

## Summary

The `self_destruct` component is the Computronics self-destruct card. It arms a countdown fuse and eventually detonates at the host position unless the host is restarted or stopped first.

## Availability

- Repository: `computronics`
- Component name: `self_destruct`
- Typical host: Computronics self-destruct card in a supported OC host

## Usage

```lua
local component = require("component")
local boom = component.self_destruct
```

## Fuse Behavior

- The fuse is stored internally in ticks.
- `start(seconds)` converts seconds to ticks using `floor(seconds * 20)`.
- Once armed, the fuse counts down every server tick.
- When it reaches zero, the card calls `SelfDestruct.goBoom(...)` at the host position.
- The fuse state is saved to NBT and survives normal saves.
- If the neighboring computer emits `computer.started` or `computer.stopped`, the fuse is reset to `-1`.

## API

### start

- Syntax: `boom.start([time])`
- Returns:
  - Success: `seconds`
  - Failure: `-1, reason`
  - Severe failure: exception
- Purpose: Arms the self-destruct fuse.

Parameters:

- `time:number` optional: desired fuse time in seconds. Defaults to `5`.

Behavior:

- If the fuse is already armed, the callback returns `-1, "fuse has already been set"`.
- Times greater than `100000` throw `time may not be greater than 100000`.

Example:

```lua
local seconds = assert(boom.start(10))
print("Fuse:", seconds)
```

### time

- Syntax: `boom.time()`
- Returns:
  - Armed: `secondsRemaining`
  - Unarmed: `-1, reason`
- Purpose: Returns the remaining fuse time in seconds.

Behavior:

- If the fuse has not been armed, the callback returns `-1, "fuse has not been set"`.

Example:

```lua
local left, reason = boom.time()
print(left, reason)
```

## Related

- `component`
- `particle`
