# computer

## Summary

This page documents the built-in OpenComputers machine component. The generated filename is `opencomputers-machine.md`, but the real component name exposed by a machine node is `computer`.

The component provides basic machine control and introspection:

- start and stop
- running-state queries
- simple audible beeps
- device inventory metadata
- known program locations

## Availability

- Repository: `opencomputers`
- Actual component name: `computer`
- Typical host: OpenComputers computer or case machine node

## Usage

```lua
local component = require("component")
local computerComponent = component.computer

if not computerComponent then
  error("computer component not available")
end
```

## API

### start

- Syntax: `computerComponent.start()`
- Returns: `boolean`
- Purpose: Starts the machine if it is currently stopped.

Behavior:

- Returns `true` only when the state actually changes.
- Returns `false` if the machine is already running, stopping, or otherwise cannot start.
- Startup can still fail internally for reasons such as missing CPU, RAM, power, or too many components. In those cases the machine emits crash feedback and the callback returns `false`.

### stop

- Syntax: `computerComponent.stop()`
- Returns: `boolean`
- Purpose: Requests a shutdown.

Behavior:

- Returns `true` only when a stop request was newly accepted.
- Returns `false` if the machine is already stopped or already stopping.

### isRunning

- Syntax: `computerComponent.isRunning()`
- Returns: `boolean`
- Purpose: Returns whether the machine is considered running.

Behavior:

- This remains `true` while paused.
- It becomes `false` only when the top machine state is `Stopped` or `Stopping`.

### beep

- Syntax:
  - `computerComponent.beep(pattern)`
  - `computerComponent.beep([frequency[, duration]])`
- Returns: no meaningful value
- Purpose: Plays an audible feedback tone or tone pattern.

Parameters:

- `pattern:string`: OpenComputers beep pattern string.
- `frequency:number` optional: tone frequency in hertz.
- `duration:number` optional: tone duration in seconds.

Behavior:

- Numeric frequency must be in the range `20` through `2000`, otherwise the callback throws `invalid frequency, must be in [20, 2000]`.
- Numeric duration is converted to milliseconds and clamped to `50` through `5000`.
- For numeric beeps, the calling context pauses for the beep duration.

Example:

```lua
computerComponent.beep(880, 0.2)
computerComponent.beep("..")
```

### getDeviceInfo

- Syntax: `computerComponent.getDeviceInfo()`
- Returns: `table`
- Purpose: Collects device metadata for visible and reachable components on the machine network.

Behavior:

- The callback pauses for `1` second because iterating all nodes can be expensive.
- The returned table is keyed by component address.
- Each value is a metadata table containing fields such as class, description, vendor, and product when the target host provides them.

### getProgramLocations

- Syntax: `computerComponent.getProgramLocations()`
- Returns: `table`
- Purpose: Returns a map from known program names to the disk label that provides them.

Behavior:

- The mapping depends on the active architecture name.

## Example

```lua
print("Running:", computerComponent.isRunning())

for address, info in pairs(computerComponent.getDeviceInfo()) do
  print(address, info.description, info.product)
end

for program, disk in pairs(computerComponent.getProgramLocations()) do
  print(program, disk)
end
```

## Related

- `component`
- `opencomputers-screen`
