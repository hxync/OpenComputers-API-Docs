# events

## Summary

Event handling in OpenComputers usually flows through `computer.pushSignal`, `computer.pullSignal`, the OpenOS `event` library, and callback-driven component signals.

## Usage

```lua
local event = require("event")
local name, address, x, y = event.pull("touch")
```

## Notes

- Signals originate from hardware, component callbacks, or software calls such as `computer.pushSignal`.
- OpenOS wraps low-level signal handling through the `event` library.
- Component pages in this documentation call out component-specific signal payloads where they are known from source or bundled manuals.
