# assembler

## Summary

The `assembler` component is the programmable interface of the Assembler block. It does not expose every recipe rule directly; instead, it lets Lua scripts check whether the current layout is ready and start the assembly job once the machine is correctly populated.

## Availability

- Repository: `opencomputers`
- Lua component name: `assembler`
- Typical host: assembler block

## What It Does

- Builds advanced OpenComputers devices such as robots, drones, tablets, and microcontrollers.
- Validates the inserted case and all installed parts against the selected template.
- Converts the inserted parts into one finished device after enough energy has been consumed.

## How Assembly Works

1. Put the base item for the target device into the first assembler slot.
2. Insert the required parts into the remaining template-controlled slots.
3. Call `status()` to see whether the current layout is valid.
4. Call `start()` to consume the parts and begin the job.
5. Wait until the job finishes; the completed device is placed back into the first slot.

Once a job has started, the assembler stores the result internally and spends energy over time until the device is complete.

## API

### status

- Syntax: `assembler.status()`
- Returns:
  - While assembling: `"busy", progressPercent`
  - While idle and valid: `"idle", true`
  - While idle and invalid: `"idle", false`
- Purpose: Reports whether the assembler is currently building something or whether the current slot layout is ready to start.

`progressPercent` is a numeric percentage in the `0` to `100` range.

### start

- Syntax: `assembler.start()`
- Returns: `boolean`
- Purpose: Starts assembling the currently configured device.

Return value meanings:

- `true`: the job was accepted and assembly started.
- `false`: the machine was already busy, the output buffer was occupied, or the current slot layout was invalid.

## Practical Notes

- The assembler can only work on one job at a time.
- You cannot start a new job while another one is in progress.
- If the current recipe is incomplete or contains invalid parts, `status()` returns `"idle", false` and `start()` returns `false`.
- Assembly speed depends on available power because the machine consumes energy gradually until the required total is paid.
- The completed device replaces the original base item in the first slot.

## Example

```lua
local component = require("component")
local assembler = component.assembler

local state, value = assembler.status()
if state == "idle" and value == true then
  assert(assembler.start(), "failed to start assembly")
else
  error("assembler recipe is not ready")
end

repeat
  os.sleep(0.5)
  state, value = assembler.status()
  if state == "busy" then
    print(("Assembly progress: %.1f%%"):format(value))
  end
until state == "idle"

print("Assembly finished. Remove the completed device from the first slot.")
```

## Related

- `disassembler`
- `robot`
- `drone`
- `tablet`
- `microcontroller`
