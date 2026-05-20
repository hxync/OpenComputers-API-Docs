# computer

## Summary

The `computer` runtime library exposes machine identity, memory, energy, signal, user, and CPU-architecture helpers to Lua programs. It is one of the core APIs available on normal OpenComputers machines and is usually loaded with `require("computer")`.

## Availability

- Repository: `opencomputers`
- Type: core runtime API

## Usage

```lua
local computer = require("computer")

print("Address:", computer.address())
print("Uptime:", computer.uptime())
print("Free memory:", computer.freeMemory())
```

## What It Is Used For

- Measuring uptime and wall-clock time.
- Checking available memory before allocating large tables or buffers.
- Pushing synthetic signals into the local event queue.
- Querying and managing authorized users.
- Reading the network energy buffer.
- Inspecting or switching supported CPU architectures on mutable processors.

## API

### realTime()

- Syntax: `computer.realTime()`
- Returns: `number`
- Purpose: Returns the real-world wall-clock timestamp in seconds.
- Notes:
  - This is based on the host JVM system clock.
  - It is useful for timeout math and timestamps that should continue to make sense outside Minecraft tick time.

### uptime()

- Syntax: `computer.uptime()`
- Returns: `number`
- Purpose: Returns how many seconds the current computer has been running since it last started.

### address()

- Syntax: `computer.address()`
- Returns: `string` or `nil`
- Purpose: Returns the current computer node address on the OpenComputers component network.
- Notes:
  - It may be `nil` very early in startup or if the machine currently has no valid node address.

### freeMemory()

- Syntax: `computer.freeMemory()`
- Returns: `number`
- Purpose: Returns the amount of Lua-visible free memory currently available.
- Notes:
  - The value is reported after internal scaling used by the active Lua architecture.
  - This is the memory budget visible to Lua code, not total JVM memory.

### totalMemory()

- Syntax: `computer.totalMemory()`
- Returns: `number`
- Purpose: Returns the total Lua-visible memory budget available to the current machine.

### pushSignal(name, ...)

- Syntax: `computer.pushSignal(name, ...)`
- Parameters:
  - `name:string`: signal name to queue.
  - `...:any`: optional signal payload values.
- Returns: `boolean`
- Purpose: Pushes a signal into the local machine signal queue.
- Typical uses:
  - Injecting custom application events.
  - Waking logic that already waits on `event.pull()`.
  - Forwarding external state changes into a shared event-driven system.

### tmpAddress()

- Syntax: `computer.tmpAddress()`
- Returns: `string` or `nil`
- Purpose: Returns the component address of the temporary filesystem mounted as the machine's temp area, if one exists.

### users()

- Syntax: `computer.users()`
- Returns: multiple `string` values
- Purpose: Returns the list of currently authorized users for this machine.
- Notes:
  - In practice you often collect them with `{computer.users()}`.

### addUser(user)

- Syntax: `computer.addUser(user)`
- Parameters:
  - `user:string`: player name to authorize.
- Returns:
  - Success: `true`
  - Failure: `nil, reason`
- Purpose: Adds a player to the authorized user list.

### removeUser(user)

- Syntax: `computer.removeUser(user)`
- Parameters:
  - `user:string`: player name to remove.
- Returns: `boolean`
- Purpose: Removes a player from the authorized user list.
- Notes:
  - Returns `false` if the user was not present.

### energy()

- Syntax: `computer.energy()`
- Returns: `number`
- Purpose: Returns the current global energy buffer available to the computer network.
- Notes:
  - If power usage is disabled in configuration, the result may be positive infinity.

### maxEnergy()

- Syntax: `computer.maxEnergy()`
- Returns: `number`
- Purpose: Returns the maximum size of the computer network's energy buffer.

### getArchitectures()

- Syntax: `computer.getArchitectures()`
- Returns: `table`
- Purpose: Returns the list of processor architectures supported by the currently installed CPU.
- Notes:
  - Mutable processors can expose multiple architectures.
  - Fixed processors usually return a single-entry list.

### getArchitecture()

- Syntax: `computer.getArchitecture()`
- Returns: `string` or nothing
- Purpose: Returns the name of the currently active processor architecture.

### setArchitecture(name)

- Syntax: `computer.setArchitecture(name)`
- Parameters:
  - `name:string`: architecture name to activate.
- Returns:
  - `true` if the architecture changed.
  - `false` if that architecture was already active.
  - `nil, "unknown architecture"` if the requested name is not supported.
- Purpose: Switches the CPU architecture on processors that allow it.
- Notes:
  - This only works on mutable processors.
  - Scripts that depend on architecture-specific behavior should verify the result.

## Example

```lua
local computer = require("computer")

print("Machine:", computer.address())
print("Running for:", computer.uptime(), "seconds")
print("Memory:", computer.freeMemory(), "/", computer.totalMemory())
print("Energy:", computer.energy(), "/", computer.maxEnergy())

computer.pushSignal("demo_event", "hello", 123)

for _, user in ipairs({computer.users()}) do
  print("Authorized:", user)
end
```

## Related

- `component`
- `event`
- `os`
- `unicode`
