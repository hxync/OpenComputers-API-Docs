# noise

## 概述

`noise` 是 Computronics 提供的轻量级多通道发声组件。它既可以立即播放一组音调，也可以先按通道缓存短音符序列，再一次性提交播放。

该组件提供 8 个通道，支持多种波形模式。相比完整的 `sound` 声卡，它更适合简单旋律、提示音和短音效。

## 可用性

- 仓库：`computronics`
- 组件名：`noise`
- 常见宿主：安装了 Computronics noise 卡的设备

## 用法

```lua
local component = require("component")
local noise = component.noise

if not noise then
  error("未安装 noise 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### channel_count

- 语法：`noise.channel_count()`
- 返回值：`number`
- 用途：返回该声卡可用的音频通道数量。

默认实现提供 `8` 个通道。

示例：

```lua
print("Noise 通道数:", noise.channel_count())
```

### modes

- 语法：`noise.modes()`
- 返回值：`table`
- 用途：返回合法波形模式的双向映射表。

返回表同时支持两种查法：

- 数字模式编号映射到小写模式名
- 小写模式名映射到数字模式编号

内置波形如下：

- `1 = square`
- `2 = sine`
- `3 = triangle`
- `4 = sawtooth`

示例：

```lua
local modes = noise.modes()
print("square 的编号:", modes.square)
print("模式 2 的名称:", modes[2])
```

### getMode

- 语法：`noise.getMode(channel)`
- 返回值：`number`
- 用途：返回指定通道当前使用的波形模式编号。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。

错误情况：

- 通道不在 `1` 到 `8` 范围内时会抛出错误。

示例：

```lua
print("1 号通道模式:", noise.getMode(1))
```

### setMode

- 语法：`noise.setMode(channel, mode)`
- 返回值：`true`
- 用途：修改指定通道的波形模式。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。
- `mode:number`：由 `noise.modes()` 返回的波形编号。

错误情况：

- 非法通道会抛出 `"invalid channel"`
- 非法模式会抛出 `"invalid mode"`

示例：

```lua
local modes = noise.modes()
assert(noise.setMode(1, modes.sine))
```

### add

- 语法：`noise.add(channel, frequency, duration[, initialDelay])`
- 返回值：`true`
- 用途：向指定通道的缓冲区追加一个音符条目。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。
- `frequency:number`：频率，单位为 Hz。
- `duration:number`：持续时间，单位为秒。
- `initialDelay:number` 可选：此音符开始前的延迟时间，单位为秒。

行为说明：

- 每个通道的缓冲区最多只能保存 `8` 个条目。
- `frequency` 必须在 `20` 到 `2000` Hz 之间。
- `duration` 会被限制在最短 `50` 毫秒、最长 `5000` 毫秒。
- `initialDelay` 会被限制在最短 `0` 毫秒、最长 `16000` 毫秒。

错误与失败：

- 非法通道会抛出 `"invalid channel"`
- 非法频率会抛出 `"invalid frequency, must be in [20, 2000]"`
- 当该通道已满时会抛出 `channel N full`

示例：

```lua
noise.setMode(1, noise.modes().square)
noise.add(1, 440, 0.15)
noise.add(1, 660, 0.15, 0.05)
noise.add(1, 880, 0.25, 0.05)
```

### clear

- 语法：`noise.clear(channel)`
- 返回值：无
- 用途：清空待处理的音符缓冲区。

行为说明：

- 尽管回调签名带有 `channel` 参数，但当前实现实际上会把所有通道的缓冲条目全部清空。
- 因此它更适合用作“整卡缓冲重置”。

示例：

```lua
noise.clear(1)
```

### process

- 语法：`noise.process()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：把当前各通道缓冲好的序列送去播放，然后清空缓冲区。

行为说明：

- 如果之前提交的缓冲仍在处理，会返回 `false, "already processing!"`
- 播放会消耗组件网络能量
- 成功后，各通道条目缓冲区都会被清空

示例：

```lua
noise.clear(1)
noise.add(1, 523.25, 0.2)
noise.add(1, 659.25, 0.2)
noise.add(1, 783.99, 0.2)

local ok, reason = noise.process()
if not ok then
  io.stderr:write("提交播放失败: " .. tostring(reason) .. "\n")
end
```

### getActiveChannels

- 语法：`noise.getActiveChannels()`
- 返回值：`number`
- 用途：返回当前仍在播放中的通道数量。

这个值统计的是已经送出播放、尚未结束的通道数。

示例：

```lua
print("正在播放的通道数:", noise.getActiveChannels())
```

### isReady

- 语法：`noise.isReady()`
- 返回值：`boolean`
- 用途：报告当前设备是否适合继续提交新的直接播放请求。

实际使用时，如果这里返回 `false`，通常应稍等一会再尝试调用 `play` 或 `process`。

示例：

```lua
while not noise.isReady() do
  os.sleep(0.05)
end
```

### play

- 语法：`noise.play(channels)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：不经过 `add` 构建的逐通道缓冲，直接立即播放最多 8 个通道的音符。

参数说明：

- `channels:table`：外层表使用 `1` 到 `8` 作为通道编号。每个值也是一个表，其中索引 `1` 是频率（Hz），索引 `2` 是持续时间（秒）。

行为说明：

- 外层表最多只能包含 `8` 个条目。
- 每个通道值都必须是 Lua 表。
- 频率必须在 `20` 到 `2000` Hz 之间。
- 持续时间省略时默认按 `0.1` 秒处理。
- 持续时间会被限制在最短 `50` 毫秒、最长 `5000` 毫秒。
- 如果某个通道当前已经忙碌，该通道这次不会被覆盖，而是直接跳过。

常见失败返回：

- `false, "already processing"`
- `false, "table must not contain more than 8 frequencies"`
- 组件能量不足时返回 `false, reason`

示例：

```lua
local ok, reason = noise.play({
  [1] = {440, 0.20},
  [2] = {660, 0.20},
  [3] = {880, 0.30}
})

if not ok then
  io.stderr:write("直接播放失败: " .. tostring(reason) .. "\n")
end
```

## 说明

- Lua 里的通道编号是从 `1` 开始的。
- `setMode` 设置的波形既会影响 `add` 加入缓冲的播放，也会影响 `play` 的直接播放。

## 相关内容

- `component`
- `beep`
- `sound`
