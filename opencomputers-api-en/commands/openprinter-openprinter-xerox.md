# xerox

## Summary

Duplicate the page currently scanned by an OpenPrinter device.

## Availability

- Repository: `openprinter`
- System: `openprinter`
- Type: `command`

## Source

- `OpenPrinter/src\main\resources\assets\openprinter\lua\printer\bin\xerox.lua`

## Syntax

```lua
xerox [COUNT]
```

## Purpose

Clear the printer buffer, scan the page currently loaded in the OpenPrinter device, copy the scanned title and line content back into the writable print buffer, and then print that rebuilt page one or more times.

## Parameters

- `COUNT`: Number of copies to print. If omitted, the command defaults to `1`. Accepted range: `1` to `9`.

## Examples

```lua
xerox
```

Print one duplicate of the page currently loaded in the printer.

```lua
xerox 3
```

Print three copies of the scanned page in sequence.

## Notes

- The script directly uses `component.openprinter` and does not provide address selection when multiple printers exist.
- The page title is copied with `printer.setTitle(pageTitle)` before each print run.
- The command assumes a printable scanned page is already present; it does not create a new document from plain text input.
