# server_destruct

## Summary

The `server_destruct` component is the rack-mounted self-destruct board from Computronics.
It exposes the same countdown API shape as the portable `self_destruct` card, but it is designed for server racks and has additional maintenance behavior while installed in a rack.

## Availability

- Repository: `computronics`
- Component name: `server_destruct`
- Typical host: Computronics rack self-destruct board mounted in an OpenComputers rack

## Usage

```lua
local component = require("component")
local boom = component.server_destruct
```

## Rack Behavior

- The board consumes ongoing maintenance power while active in a rack.
- If it cannot pay that maintenance cost, it deactivates itself and clears the current fuse.
- The armed state is saved to NBT and survives normal saves.
- If the neighboring computer emits `computer.started` or `computer.stopped`, the fuse is reset.

## Explosion Behavior

Unlike the portable `self_destruct` card, the rack version does not only destroy its immediate host.
When the fuse reaches zero, it performs a breadth-first search through nearby racks and queues chained explosions across the connected rack cluster.

Important details from the implementation:

- connected racks are searched outward up to a squared distance of `256`
- explosions are processed in batches instead of all at once
- each batch is exploded every few server ticks

This makes `server_destruct` substantially more destructive than the normal card.

## API

### `start`

- Syntax: `boom.start([time])`
- Returns:
  - Success: `seconds`
  - Failure: `-1, reason`
  - Severe failure: callback error
- Purpose: arms the rack self-destruct fuse

Parameters:

- `time:number` optional: desired fuse time in seconds, default `5`

Behavior:

- if the fuse is already armed, the callback returns `-1, "fuse has already been set"`
- if `time > 100000`, the callback throws `time may not be greater than 100000`
- the stored timer uses ticks internally and is derived from `floor(time * 20)`

Example:

```lua
local seconds = assert(boom.start(10))
print("Fuse:", seconds)
```

### `time`

- Syntax: `boom.time()`
- Returns:
  - Armed: `secondsRemaining`
  - Unarmed: `-1, reason`
- Purpose: returns the remaining fuse time in seconds

Behavior:

- if the fuse is not armed, the callback returns `-1, "fuse has not been set"`

Example:

```lua
local left, reason = boom.time()
print(left, reason)
```

## Comparison With `self_destruct`

- `self_destruct` is a portable card-style component for ordinary hosts.
- `server_destruct` is a rack board component.
- Both expose `start()` and `time()`.
- `server_destruct` can propagate destruction through nearby racks and depends on rack maintenance power.

## Related

- `self_destruct`
- `particle`
