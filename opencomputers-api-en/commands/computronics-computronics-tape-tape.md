# tape

## Summary

Operate a Computronics tape drive, inspect or modify tape metadata, and write audio bytes onto a tape.

## Availability

- Repository: `computronics`
- System: `computronics-tape`
- Type: `command`

## Source

- `Computronics/src\main\resources\assets\computronics\loot\tape\usr\bin\tape.lua`
- `Computronics/src\main\resources\assets\computronics\doc\opencomputers\computronics\en_US\item\tape.md`
- `Computronics/src\main\resources\assets\computronics\doc\opencomputers\computronics\zh_CN\item\tape.md`

## Syntax

```lua
tape play
```

```lua
tape pause
```

```lua
tape stop
```

```lua
tape rewind
```

```lua
tape wipe
```

```lua
tape label [NAME]
```

```lua
tape speed SPEED
```

```lua
tape volume VOLUME
```

```lua
tape write SOURCE
```

## Purpose

Control playback state, rewind and erase the inserted tape, read or change the tape label, adjust playback speed and volume, and stream new tape contents from either a local file or an HTTP URL into the selected tape drive.

## Parameters

- `NAME`: Optional tape label. If omitted with `label`, the command prints the current label or reports that the tape is unlabeled.
- `SPEED`: Playback speed. Accepted range: `0.25` to `2.0`.
- `VOLUME`: Playback volume. Accepted range: `0.0` to `1.0`.
- `SOURCE`: Local file path or `http://` / `https://` URL whose raw bytes should be written onto the tape.

## Options

- `--address=ADDRESS`: Use a specific `tape_drive` component instead of the default primary one.
- `--b=BYTES`: Chunk size used while copying data to tape. Default: `2048` bytes.
- `--t=SECONDS`: Maximum connection wait when writing from a URL. Default: `5` seconds.
- `-y`: Skip confirmation prompts before destructive `wipe` and `write` operations.

## Examples

```lua
tape play
```

Start playback on the inserted tape if it is not already playing.

```lua
tape label DemoTape
```

Set the tape label to `DemoTape`.

```lua
tape write /home/music.dfpwm
```

Copy the contents of `/home/music.dfpwm` onto the tape.

```lua
tape --address=123 write https://example.com/music.dfpwm
```

Download bytes from an HTTP source and write them to the explicitly selected tape drive.

## Notes

- A `tape_drive` component must exist, and the chosen drive must already contain a ready tape.
- `pause` stops playback without rewinding, while `stop` both stops playback and rewinds the tape to the beginning.
- `wipe` overwrites the full tape with `0xAA` bytes in `8192`-byte blocks, then rewinds to the start.
- If the source data is larger than the tape capacity, the script truncates the write to the tape's maximum size.
- Writing from a URL additionally requires an `internet` component.
