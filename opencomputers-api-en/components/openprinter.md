# openprinter

## Summary

The `openprinter` component controls the OpenPrinter page printer. It can queue page lines in memory, set a title, print a finished page, print name tags when enabled, inspect printed pages placed in the scanner slot, and report the remaining paper and ink supplies.

## Availability

- Repository: `openprinter`
- Component name: `openprinter`
- Typical host: placed OpenPrinter printer block

## Usage

```lua
local component = require("component")
local printer = component.openprinter

if not printer then
  error("openprinter component not installed")
end
```

The printer also mounts a bundled filesystem environment named `printer` from its internal Lua resources.

## Inventory Layout

- Slot `0`: black ink cartridge
- Slot `1`: color ink cartridge
- Slot `2`: input paper, paper roll, or name tag
- Slots `3` through `12`: output slots
- Slot `13`: scanner slot for reading an existing printed page

Practical implications:

- Printing pages needs both black and color ink to be present.
- Printing name tags only needs black ink plus a vanilla name tag in slot `2`.
- `scan()` and `scanLine()` only read the item placed in slot `13`.

## Page Buffer Model

`writeln()` does not print immediately. It appends lines to an in-memory page buffer:

- maximum line count: `20`
- each stored line also records:
  - text
  - integer color
  - alignment string

`setTitle()` stores the title for the next printed page. `print()` consumes the current buffered page and clears the line buffer after a successful print.

## Ink And Paper Notes

- `OpenPrinter.cfg.printerInkUse` defaults to `4000` uses per cartridge.
- Paper can be supplied as:
  - vanilla paper items
  - OpenPrinter paper rolls
- A paper roll reports remaining units as `256 - itemDamage`.

## API

### greet

- Syntax: `printer.greet()`
- Returns: `string`
- Purpose: Returns the built-in OpenPrinter greeting.

Returned text:

```text
Lasciate ogne speranza, voi ch'intrate
```

### writeln

- Syntax:
  - `printer.writeln(text)`
  - `printer.writeln(text, color)`
  - `printer.writeln(text, alignment)`
  - `printer.writeln(text, color, alignment)`
- Returns: `true`
- Purpose: Appends one line to the pending page buffer.

Parameters:

- `text:string`: line contents.
- `color:number` optional: line color. Defaults to `0x000000`.
- `alignment:string` optional: alignment marker string. Defaults to `"left"`.

Behavior:

- Throws `To many lines.` when the buffer already contains `20` lines.
- The implementation stores alignment exactly as provided and does not validate the string.

Example:

```lua
printer.writeln("Quarterly Report", 0x3366CC, "center")
printer.writeln("Line 2")
```

### setTitle

- Syntax: `printer.setTitle(title)`
- Returns: `true`
- Purpose: Sets the title for the next printed page.

Parameters:

- `title:string`: page title and display name to apply to the next printed page.

Behavior:

- The title is consumed on the next successful `print()`.

Example:

```lua
printer.setTitle("Meeting Notes")
```

### print

- Syntax: `printer.print([unused, copies])`
- Returns:
  - Success: `true`
  - Fallback path: `false`
  - Failure: exception
- Purpose: Creates one printed page in the first empty output slot.

Parameters:

- `unused:any` optional: ignored by the implementation.
- `copies:number` optional: read from the second argument and defaults to `9`.

Important behavior:

- The current implementation returns after creating the first page, so one call effectively prints one page even if a larger copy count is supplied.
- The buffered lines, colors, and alignments are cleared after the successful page is created.
- The stored title is also cleared after being applied.

Required supplies:

- black ink in slot `0`
- color ink in slot `1`
- paper or paper roll in slot `2`
- at least one empty output slot from `3` to `12`

Common failures:

- `Please load Ink.`
- `Please load Paper.`
- `No empty output slots.`

Example:

```lua
printer.clear()
printer.setTitle("Memo")
printer.writeln("Hello world")
assert(printer.print())
```

### printTag

- Syntax: `printer.printTag(name)`
- Returns: `true`
- Purpose: Prints one named vanilla name tag into the first empty output slot.

Parameters:

- `name:string`: display name written into the new name tag.

Requirements:

- `enableNameTag` must be enabled in the OpenPrinter config.
- black ink must be loaded in slot `0`
- a vanilla name tag must be loaded in slot `2`
- at least one output slot from `3` to `12` must be empty

Common failures:

- `Name Tag printing is disabled.`
- `Please load Black Ink.`
- `Please load Name Tags.`
- `No empty output slots.`

Example:

```lua
assert(printer.printTag("Warehouse Key"))
```

### scanLine

- Syntax: `printer.scanLine(index)`
- Returns:
  - Success: `string`
  - Invalid scanned page: `false`
  - Empty scanner slot: callback error
- Purpose: Reads one stored line from the printed page in scanner slot `13`.

Parameters:

- `index:number`: line index to read.

Behavior:

- The printed page stores line keys as `line0` through `line20`.
- If slot `13` contains an item that is not a tagged `PrintedPage`, the callback returns `false`.
- If slot `13` is empty, the current implementation dereferences the slot without a null guard, so the callback errors instead of returning `false`.

Example:

```lua
local line = printer.scanLine(0)
print(line)
```

### scan

- Syntax: `printer.scan()`
- Returns:
  - Success: `title, lines`
  - Invalid scanned page: `false`
  - Empty scanner slot: callback error
- Purpose: Reads the scanned printed page from slot `13`.

Returned values:

- `title:string|nil`: stored page title
- `lines:table`: numeric table of stored raw line strings

Behavior:

- The line values are returned exactly as saved in NBT.
- Each raw line string includes embedded separators joining:
  - text
  - color
  - alignment
- If slot `13` contains an item that is not a printed page, the callback returns `false`.
- If slot `13` is empty, the implementation dereferences the stack before checking it, so the callback errors instead of returning `false`.

Example:

```lua
local title, lines = printer.scan()
if title ~= false then
  print(title)
  for index, raw in pairs(lines) do
    print(index, raw)
  end
end
```

### getPaperLevel

- Syntax: `printer.getPaperLevel()`
- Returns:
  - Paper loaded: `number`
  - Missing or invalid input: `false`
- Purpose: Reports the remaining paper supply in input slot `2`.

Behavior:

- For a paper roll, returns `256 - itemDamage`.
- For a normal paper stack, returns the current stack size.

### getBlackInkLevel

- Syntax: `printer.getBlackInkLevel()`
- Returns:
  - Valid black cartridge: `number`
  - Missing or wrong item: `false`
- Purpose: Reports remaining black ink uses.

Behavior:

- Returns `printerInkUse - itemDamage`.

### getColorInkLevel

- Syntax: `printer.getColorInkLevel()`
- Returns:
  - Valid color cartridge: `number`
  - Missing or wrong item: `false`
- Purpose: Reports remaining color ink uses.

Behavior:

- Returns `printerInkUse - itemDamage`.

### charCount

- Syntax: `printer.charCount(text)`
- Returns: `number`
- Purpose: Counts visible characters while ignoring Minecraft formatting codes.

Parameters:

- `text:string`: text to measure.

Behavior:

- The implementation strips formatting sequences matching `§` color/style codes before counting.

Example:

```lua
print(printer.charCount("§aGreen Text"))
```

### clear

- Syntax: `printer.clear()`
- Returns: `true`
- Purpose: Clears the pending line buffer, color buffer, alignment buffer, and pending title.

Example:

```lua
printer.clear()
```

## Example

```lua
printer.clear()
printer.setTitle("Status Report")
printer.writeln("System Online", 0x00AA00, "left")
printer.writeln("All nodes reachable", 0x000000, "left")

print("Paper:", printer.getPaperLevel())
print("Black ink:", printer.getBlackInkLevel())
print("Color ink:", printer.getColorInkLevel())

assert(printer.print())
```

## Related

- `component`
- `openprinter-openprinter-xerox`
