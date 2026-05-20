# sandbox

## Summary

Build a small Plan9k sandbox pipeline with process spawning, cgroups, and redirected standard streams.

## Availability

- Repository: `opencomputers`
- System: `plan9k`
- Type: `command`

## Source

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sandbox.lua`

## Syntax

```lua
sandbox [SUBCOMMAND...]
```
```lua
sandbox module spawn FILE [args N ARG...] join
```

## Purpose

Interpret a sequence of sandbox subcommands from left to right. You can create module and component namespaces, whitelist or blacklist specific components, redirect stdin/stdout/stderr to `/dev/null`, spawn one or more processes, and optionally wait for the last spawned process.

## Parameters

- `spawn FILE`: Spawn a process from the given script or executable path.
- `args N ARG...`: After `spawn FILE`, pass exactly `N` following values as process arguments.
- `join`: Wait for the last process spawned in the current sandbox command line.
- `module`: Create a new module namespace/cgroup before later spawns.
- `component`: Create a component access cgroup using the current whitelist and blacklist.
- `wl ADDRESS`: Add a component address to the whitelist used by the next `component` step.
- `bl ADDRESS`: Add a component address to the blacklist used by the next `component` step.
- `quietin`: Replace standard input with `/dev/null` for later spawns.
- `quietout`: Replace standard output with `/dev/null` for later spawns.
- `quieterr`: Replace standard error with `/dev/null` for later spawns.

## Options

- `-h, --help`: Print the built-in help text with examples.

## Examples

```lua
sandbox module spawn /bin/a.lua join
```
Run `/bin/a.lua` in a fresh module namespace and wait for it to finish.

```lua
sandbox wl 0ab component spawn /bin/a.lua
```
Spawn `/bin/a.lua` with a component access cgroup that only explicitly allows the whitelisted component list.

```lua
sandbox quietout quieterr spawn /bin/a.lua
```
Start `/bin/a.lua` with both stdout and stderr silenced.

## Notes

- The parser is sequential and stateful: `wl` and `bl` collect addresses until the next `component` step consumes them.
- Only the last spawned process can be waited on via `join`.
