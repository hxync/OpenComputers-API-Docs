# pipe

## Summary

Coroutine-based pipeline helpers used to connect OpenOS processes through read/write streams.

## Availability

- Repository: `opencomputers`
- System: `openos`
- Type: `library`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\pipe.lua`

## Usage

```lua
local pipe = require("pipe")
```

## API

### pipe.buildPipeChain(progs)

- Description: Link a list of loaded process threads into `prog1 | prog2 | ...` style pipe connections.
- Parameters:
  - `progs`: Array of process root coroutines in pipeline order.

```lua
pipe.buildPipeChain({producer, consumer})
```

### pipe.createCoroutineStack(root, env, name)

- Description: Wrap a root coroutine or function in a coroutine handler that can yield across pipe boundaries safely.
- Parameters:
  - `root`: Existing process coroutine or function entry point to wrap.
  - `env`: Optional environment table used when `root` is a function and must be loaded as a process.
  - `name`: Optional process name used when loading a function root.

```lua
local stack = pipe.createCoroutineStack(threadRoot)
```

### pipe.popen(prog, mode, env)

- Description: Start one command behind a controller stream and return a buffered handle for reading from or writing to it.
- Parameters:
  - `prog`: Shell command line to execute.
  - `mode`: Either `"r"` to read command output or `"w"` to write command input.
  - `env`: Optional environment table passed into the spawned shell execution.

```lua
local handle = pipe.popen("ls /etc", "r")
```

## Notes

- The implementation builds dedicated coroutine stacks so natural yields and pipe reads can coexist correctly across chained commands.
