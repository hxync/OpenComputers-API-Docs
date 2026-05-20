# note

## 概述

在音名、MIDI 值、频率、音符盒刻度和简单蜂鸣播放之间进行转换。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\note.lua`

## 用法

```lua
local note = require("note")
```

## 接口

### note.freq(n)

- 说明: 把音名或 MIDI 值转换成蜂鸣频率。
- 参数:
  - `n`

```lua
note.freq(nil)
```

### note.midi(n)

- 说明: 把音名或频率转换成 MIDI 音符编号。
- 参数:
  - `n`

```lua
note.midi(nil)
```

### note.name(n)

- 说明: 把 MIDI 音符编号还原成类似 `A#4` 的音名。
- 参数:
  - `n`

```lua
note.name(nil)
```

### note.play(tone,duration)

- 说明: 通过 `computer.beep` 立即播放一个音符。
- 参数:
  - `tone`
  - `duration`

```lua
note.play(nil, nil)
```

### note.ticks(n)

- 说明: 在音符盒刻度 `0..24` 与对应的 MIDI 值 `34..58` 之间互相转换。
- 参数:
  - `n`

```lua
note.ticks(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
