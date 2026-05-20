# tape_drive

## Summary

Autorun helper that registers shell completion for the Computronics `tape` command.

## Availability

- Repository: `computronics`
- System: `computronics`
- Type: `example`

## Source

- `Computronics/src\main\resources\assets\computronics\lua\peripheral\tape_drive\autorun\tape_drive`

## Notes

- It appends `/rom/programs/tape_drive` to the shell search path.
- The completion function suggests tape subcommands such as `play`, `stop`, `pause`, `rewind`, `label`, `speed`, `volume`, and `write`, and it delegates the second argument of `write` to filesystem path completion.
