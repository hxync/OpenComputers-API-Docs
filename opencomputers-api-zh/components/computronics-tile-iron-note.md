# iron_noteblock

## 概述

`iron_noteblock` 是 Computronics 提供的可编程音符发射组件。Lua 可以让它使用显式指定的乐器，或者使用方块默认乐器判定来播放音符。

它适合做警报声、简单旋律、bundled 线路音乐触发器以及机器反馈音。

## 可用性

- 仓库：`computronics`
- 组件名：`iron_noteblock`
- 常见宿主：放置在世界中的 Computronics 铁音符方块

## 用法

```lua
local component = require("component")
local note = component.iron_noteblock

if not note then
  error("未安装 iron_noteblock 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 乐器

该组件既接受数字乐器编号，也接受字符串乐器名。

支持的乐器名称：

- `harp`
- `bd`
- `snare`
- `hat`
- `bassattack`
- `pling`
- `bass`

数字乐器编号会按 `7` 取模循环。

如果不传乐器，方块会像原版音符盒一样，根据下方方块材质自动选择默认乐器。

## 接口

### playNote

- 语法：`note.playNote([instrument,] note[, volume])`
- 返回值：没有可依赖的有效返回值
- 用途：把一个音符加入待播放队列，并在下一次方块更新时实际播放。

参数说明：

- `instrument:number|string|nil` 可选：
  - 数字表示乐器编号
  - 字符串表示支持的乐器名
  - `nil` 表示在三参数形式下仍然强制使用默认乐器
- `note:number`：音高值，必须大于等于 `0`
- `volume:number` 可选：`0` 到 `1` 的归一化音量

行为说明：

- 省略 `volume` 时默认使用 `1.0`
- Lua 层的归一化音量会在内部转换成方块真实播放音量
- 非法音量会抛出错误，例如：
  - `bad argument #N (number between 0 and 1 expected, got X)`
- 非法乐器名会抛出：
  - `invalid instrument: name`
- 负数音高会抛出：
  - `invalid note: value`

示例：使用默认乐器。

```lua
note.playNote(12)
```

示例：显式使用字符串乐器名。

```lua
note.playNote("pling", 18, 0.8)
```

示例：使用数字乐器编号。

```lua
note.playNote(3, 10, 0.5)
```

示例：在三参数形式下强制使用默认乐器。

```lua
note.playNote(nil, 7, 1.0)
```

## 说明

- OpenComputers 侧这个回调只是把播放任务压入队列，不会返回可靠的成功标记。
- 这个方块也可以响应 bundled 线路输入，根据位图直接播放音符，而不必经过 Lua。

## 相关内容

- `component`
- `sound`
