# tape_rewind

## 概述

本页记录的是 Computronics 磁带机在 OpenComputers 中暴露出来的组件接口。虽然生成出的文件名是 `tape-rewind.md`，但这个方块在 OpenComputers 里真正提供的组件名其实是 `tape_drive`。

它可以读写磁带数据、移动磁头位置、控制播放，并调节播放速度与音量。

## 可用性

- 仓库：`computronics`
- 实际组件名：`tape_drive`
- 常见宿主：放置在世界中的 Computronics 磁带机

## 用法

```lua
local component = require("component")
local tape = component.tape_drive

if not tape then
  error("未安装 tape_drive 组件")
end
```

## 磁带基础

- 该驱动器一次只能插入一盘盒式磁带。
- 磁带既可以存音频，也可以存任意字节数据。
- 音频播放底层使用的是 DFPWM 数据包。
- 如果没有连接外部音频接收器，磁带机会使用自己的内置扬声器播放。

## 播放状态

`getState()` 会返回以下字符串之一：

- `"STOPPED"`
- `"PLAYING"`
- `"REWINDING"`
- `"FORWARDING"`

说明：

- Lua API 直接提供的是 `play()` 和 `stop()`。
- 快速倒带和快进在内部确实存在状态，但 Lua 层主要通过 `seek()` 来移动磁头位置，而不是直接切换这两个状态。

## 接口

### isEnd

- 语法：`tape.isEnd()`
- 返回：`boolean`
- 用途：判断驱动器是否为空，或当前磁头是否已经到达磁带末尾。

行为说明：

- 当没有插入磁带时，也会返回 `true`。

### isReady

- 语法：`tape.isReady()`
- 返回：`boolean`
- 用途：判断当前是否插入了磁带。

### getSize

- 语法：`tape.getSize()`
- 返回：`number`
- 用途：返回磁带总容量，单位是字节。

行为说明：

- 没有磁带时返回 `0`。

### getPosition

- 语法：`tape.getPosition()`
- 返回：`number`
- 用途：返回当前磁头位置，单位是字节。

行为说明：

- 没有磁带时返回 `0`。

### setLabel

- 语法：`tape.setLabel(label)`
- 返回：`string|nil`
- 用途：设置磁带存储在 NBT 中的标签。

参数说明：

- `label:string`：新的磁带标签。空字符串会移除已有标签。

行为说明：

- 成功时返回修改后的标签。
- 没有磁带时返回 `nil`。

### getLabel

- 语法：`tape.getLabel()`
- 返回：`string|nil`
- 用途：返回当前磁带标签。

### seek

- 语法：`tape.seek(length)`
- 返回：`number|nil`
- 用途：按有符号字节偏移移动磁头。

参数说明：

- `length:number`：要移动的字节数。负数表示倒带。

行为说明：

- 返回实际移动成功的字节数。
- 没有磁带时返回 `nil`。
- 实际移动量会被磁带起点和终点限制。

示例：

```lua
print("实际移动：", tape.seek(-1024))
```

### read

- 语法：
  - `tape.read()`
  - `tape.read(length)`
- 返回：
  - 没有磁带：`nil`
  - 不带长度参数：`number`
  - 带长度参数：`string`
- 用途：从当前磁头位置读取数据。

行为说明：

- `tape.read()` 会读取单个字节，并以 `0` 到 `255` 的数字返回。
- `tape.read(length)` 会读取多个字节，并把这些字节构造成 Lua 字符串返回。
- OpenComputers 回调接受任意非负整数长度。
- 底层读取会在磁带末尾自动停止。

示例：

```lua
local byte = tape.read()
local chunk = tape.read(128)
print(byte, #chunk)
```

### write

- 语法：`tape.write(data)`
- 返回：无可依赖的有效返回值
- 用途：向磁带写入一个字节，或写入一段字节串。

参数说明：

- `data:number|string`

行为说明：

- 传入数字时写入一个字节。
- 传入字符串时写入该字符串的原始字节。
- 没有磁带时，该回调不会执行任何写入。
- 传错类型会抛出 `bad arguments #1 (number or string expected)`。

### play

- 语法：`tape.play()`
- 返回：`boolean`
- 用途：开始播放音频。

行为说明：

- 只有在磁带已插入且状态成功变为 `PLAYING` 时，才返回 `true`。

### stop

- 语法：`tape.stop()`
- 返回：`boolean`
- 用途：停止音频播放。

行为说明：

- 只有在磁带已插入且状态成功变为 `STOPPED` 时，才返回 `true`。

### setSpeed

- 语法：`tape.setSpeed(speed)`
- 返回：`boolean`
- 用途：设置播放速度。

参数说明：

- `speed:number`：有效范围是 `0.25` 到 `2.0`

行为说明：

- 内部会把音频包大小设置为 `round(1024 * speed)`。
- 合法时返回 `true`，超出范围时返回 `false`。

### setVolume

- 语法：`tape.setVolume(volume)`
- 返回：无可依赖的有效返回值
- 用途：设置播放音量。

参数说明：

- `volume:number`：目标音量

行为说明：

- 小于 `0` 的值会被限制为 `0`。
- 大于 `1` 的值会被限制为 `1`。
- 内部会再把这个值缩放到 `0` 到 `127` 的范围。

### getState

- 语法：`tape.getState()`
- 返回：`string`
- 用途：返回当前驱动器状态字符串。

## 示例

```lua
if not tape.isReady() then
  error("请先插入磁带")
end

print("容量：", tape.getSize())
print("位置：", tape.getPosition())

tape.setLabel("music-a")
tape.setSpeed(1.0)
tape.setVolume(0.5)
tape.seek(0)
tape.play()
os.sleep(1)
tape.stop()
```

## 相关内容

- `component`
- `computronics-computronics-tape-tape`
