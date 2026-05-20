# cardwriter

## Summary

Interactively write payload data, a display name, and a lock flag to an OpenSecurity magstripe or RFID card.

## Availability

- Repository: `opensecurity`
- System: `cardwriter`
- Type: `command`

## Source

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\cardwriter\bin\cardwriter.lua`

## Syntax

```lua
cardwriter
```

## Purpose

Clear the terminal, prompt for three pieces of information, and then write them to the inserted card through the `os_cardwriter` component: the raw card payload, the card display name, and whether the card should be locked after writing.

## Interactive Fields

- `Card Data`: Raw data string stored on the card.
- `Card Name`: Display label written to the card.
- `Lock Card`: Whether the finished card should be locked against later edits.

## Examples

```lua
cardwriter
```

Open the interactive prompts and write one card through the attached OpenSecurity card writer.

## Notes

- The command is fully interactive and does not accept command-line arguments.
- The implementation writes through `component.os_cardwriter`, so that specific component must be available.
- Although the prompt says `[y/N]`, pressing Enter without typing anything still locks the card.
- After writing, the script clears the terminal again instead of printing a success summary.
