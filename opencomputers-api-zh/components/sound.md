# sound

## 概述

`sound` 是 Computronics 提供的可编程声卡组件。它提供真正的指令队列、8 个音频通道、多种波形、音量控制、延迟以及基础调制能力。

当你需要的功能超过简单蜂鸣或短促音调时，就应该使用它。脚本通常先构建一段音频指令队列，再通过 `process()` 一次性提交播放。

## 可用性

- 仓库：`computronics`
- 组件名：`sound`
- 常见宿主：安装了 Computronics sound 声卡的设备

## 用法

```lua
local component = require("component")
local sound = component.sound

if not sound then
  error("未安装 sound 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 核心行为

- 默认提供 `8` 个通道。
- Lua 中的通道编号从 `1` 开始。
- 大多数方法不会立刻生效，而是把指令加入内部构建队列。
- 构建队列默认最多可容纳 `1024` 条指令。
- 队列中的延迟单位为毫秒，累计延迟默认不能超过 `5000` 毫秒。
- 必须调用 `sound.process()` 才会把队列真正送去播放。

`sound.modes()` 返回的波形模式：

- `1 = square`
- `2 = sine`
- `3 = triangle`
- `4 = sawtooth`
- `"noise" = -1` 且 `-1 = "noise"`，用于白噪声

## 接口

### channel_count

- 语法：`sound.channel_count()`
- 返回值：`number`
- 用途：返回可用音频通道数量。

示例：

```lua
print("Sound 通道数:", sound.channel_count())
```

### modes

- 语法：`sound.modes()`
- 返回值：`table`
- 用途：返回支持的波形模式双向映射表。

示例：

```lua
local modes = sound.modes()
print("sine 的编号:", modes.sine)
print("模式 4 的名称:", modes[4])
print("noise 的编号:", modes.noise)
```

### setTotalVolume

- 语法：`sound.setTotalVolume(volume)`
- 返回值：无
- 用途：立即设置整张声卡的总输出音量。

参数说明：

- `volume:number`：目标音量。小于 `0` 会被钳制到 `0`，大于 `1` 会被钳制到 `1`。

这个接口不是排队指令，而是直接影响整张声卡。

示例：

```lua
sound.setTotalVolume(0.5)
```

### clear

- 语法：`sound.clear()`
- 返回值：无
- 用途：清空当前尚未提交的构建队列。

它不会停止已经通过 `process()` 提交出去的声音，只会移除等待提交的指令。

示例：

```lua
sound.clear()
```

### open

- 语法：`sound.open(channel)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：向队列中加入“打开通道门控”的指令，使该通道可以发声。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。

如果构建队列已满，会返回 `false, "too many instructions"`。

示例：

```lua
assert(sound.open(1))
```

### close

- 语法：`sound.close(channel)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：向队列中加入“关闭通道门控”的指令，停止该通道继续输出声音。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。

示例：

```lua
assert(sound.close(1))
```

### setWave

- 语法：`sound.setWave(channel, type)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次波形切换。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。
- `type:number`：来自 `sound.modes()` 的波形编号。

特殊取值：

- `1` 方波
- `2` 正弦波
- `3` 三角波
- `4` 锯齿波
- `-1` 白噪声

错误情况：

- 非法通道会抛出错误
- 非法模式会抛出 `invalid mode: <value>`

示例：

```lua
sound.setWave(1, sound.modes().sine)
```

### setFrequency

- 语法：`sound.setFrequency(channel, frequency)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次频率修改。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。
- `frequency:number`：频率，单位 Hz。

和较简单的 `beep`、`noise` 接口不同，这个回调在 Lua 层不会主动把频率限制到固定区间，而是按给定值直接写入指令。

示例：

```lua
sound.setFrequency(1, 440)
```

### setLFSR

- 语法：`sound.setLFSR(channel, initial, mask)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次 LFSR 伪随机噪声发生器设置。

参数说明：

- `channel:number`：通道编号，从 `1` 开始。
- `initial:number`：初始寄存器状态。
- `mask:number`：反馈掩码。

适合制作复古风格的伪随机噪声，而不是周期性波形。

示例：

```lua
sound.setLFSR(2, 1, 0xB400)
```

### delay

- 语法：`sound.delay(duration)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：在声音程序中插入一段暂停。

参数说明：

- `duration:number`：延迟时长，单位为毫秒。

行为说明：

- `duration` 默认必须在 `0` 到 `5000` 毫秒之间。
- 如果累计排队延迟将超过配置上限，会返回 `false, "too many delays in queue"`。

错误情况：

- 非法数值会抛出 `invalid duration. must be between 0 and 5000`

示例：

```lua
sound.delay(250)
```

### setFM

- 语法：`sound.setFM(channel, modIndex, intensity)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次频率调制设置。

参数说明：

- `channel:number`：被调制的目标通道。
- `modIndex:number`：作为调制器的通道编号。
- `intensity:number`：调制强度。

调制器通道编号同样使用 Lua 的 `1` 基编号。

示例：

```lua
sound.setFM(1, 2, 0.5)
```

### resetFM

- 语法：`sound.resetFM(channel)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次移除频率调制的指令。

参数说明：

- `channel:number`：目标通道。

示例：

```lua
sound.resetFM(1)
```

### setAM

- 语法：`sound.setAM(channel, modIndex)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次振幅调制设置。

参数说明：

- `channel:number`：被调制的目标通道。
- `modIndex:number`：作为振幅调制器的通道编号。

示例：

```lua
sound.setAM(1, 2)
```

### resetAM

- 语法：`sound.resetAM(channel)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次移除振幅调制的指令。

参数说明：

- `channel:number`：目标通道。

示例：

```lua
sound.resetAM(1)
```

### setADSR

- 语法：`sound.setADSR(channel, attack, decay, attenuation, release)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次 ADSR 包络设置。

参数说明：

- `channel:number`：目标通道。
- `attack:number`：起音阶段时长，单位毫秒。
- `decay:number`：衰减阶段时长，单位毫秒。
- `attenuation:number`：持续阶段衰减值，通常使用 `0` 到 `1`。
- `release:number`：释音阶段时长，单位毫秒。

如果该通道之前已经有包络，底层实现会尽量保留之前的包络进度，使更新更平滑。

示例：

```lua
sound.setADSR(1, 20, 80, 0.6, 120)
```

### resetEnvelope

- 语法：`sound.resetEnvelope(channel)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次移除 ADSR 包络的指令。

参数说明：

- `channel:number`：目标通道。

示例：

```lua
sound.resetEnvelope(1)
```

### setVolume

- 语法：`sound.setVolume(channel, volume)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：为指定通道排入一次独立音量修改。

参数说明：

- `channel:number`：目标通道。
- `volume:number`：期望音量。

行为说明：

- 该值在内部会被钳制到 `0` 到 `1` 范围。

示例：

```lua
sound.setVolume(1, 0.75)
```

### process

- 语法：`sound.process()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reasonOrTime`
- 用途：把当前排好的声音指令程序提交到播放队列。

行为说明：

- 即使当前构建队列为空，也会返回 `true`
- 能量消耗与队列中的总延迟时长有关
- 成功后，当前构建队列会被转移到播放队列
- 如果上一次播放队列仍未空闲，会返回 `false, elapsedMilliseconds`，第二个返回值表示内部超时点已经过去了多少毫秒
- 组件网络能量不足时会返回 `false, "not enough energy"`

示例：播放一个短音。

```lua
sound.clear()
sound.open(1)
sound.setWave(1, sound.modes().square)
sound.setFrequency(1, 440)
sound.setVolume(1, 0.8)
sound.delay(200)
sound.close(1)

local ok, reason = sound.process()
if not ok then
  io.stderr:write("提交 sound 队列失败: " .. tostring(reason) .. "\n")
end
```

示例：播放一个双音和弦。

```lua
sound.clear()
sound.open(1)
sound.open(2)
sound.setWave(1, sound.modes().sine)
sound.setWave(2, sound.modes().sine)
sound.setFrequency(1, 523.25)
sound.setFrequency(2, 659.25)
sound.setVolume(1, 0.6)
sound.setVolume(2, 0.6)
sound.delay(300)
sound.close(1)
sound.close(2)
assert(sound.process())
```

## 说明

- 大多数回调只是把指令加入队列，只有 `process()` 成功后才会真正听到声音。
- 如果只需要一次性短音，`beep` 或 `noise` 往往更容易使用。

## 相关内容

- `component`
- `beep`
- `noise`
