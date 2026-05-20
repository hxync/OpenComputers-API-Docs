# speech_box

## 概述

`speech_box` 是 Computronics 提供的放置式文本转语音方块组件。它可以把文本转换成语音音频，并优先通过相邻的音频接收器播放；如果周围没有可用接收器，就使用自身内置扬声器发声。

它适合做会说话的终端、语音警报器、公告系统以及音频路由场景。

## 可用性

- 仓库：`computronics`
- 组件名：`speech_box`
- 常见宿主：放置在世界中的 Computronics 语音盒方块

## 用法

```lua
local component = require("component")
local speechBox = component.speech_box

if not speechBox then
  error("未安装 speech_box 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 音频行为

- 组件同一时间只能播报一段文本。
- 如果周围存在兼容的音频接收器，语音会优先送到这些接收器。
- 如果没有兼容接收器，方块会使用自己的内置扬声器播放。
- 音量以 `0` 到 `1` 的归一化值保存，内部会再转换到底层音频引擎所需范围。

## 接口

### say

- 语法：`speechBox.say(text)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：开始朗读指定文本。

参数说明：

- `text:string`：要转换成语音的文本。

行为说明：

- 如果当前已经在播报，会返回 `false, "already processing"`。
- 文本长度受配置限制，默认上限是 `300` 个字符。
- 如果当前没有可用的 TTS 后端，会返回 `false, "text-to-speech system not available"`。

示例：

```lua
local ok, reason = speechBox.say("Welcome to the facility.")
if not ok then
  io.stderr:write("语音播报失败: " .. tostring(reason) .. "\n")
end
```

### stop

- 语法：`speechBox.stop()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：停止当前正在播报的语句。

如果当前处于空闲状态，会返回 `false, "not talking"`。

示例：

```lua
speechBox.stop()
```

### isProcessing

- 语法：`speechBox.isProcessing()`
- 返回值：`boolean`
- 用途：返回当前语音盒是否仍在生成或输出语音音频。

示例：

```lua
while speechBox.isProcessing() do
  os.sleep(0.1)
end
```

### setVolume

- 语法：`speechBox.setVolume(volume)`
- 返回值：没有可依赖的有效返回值
- 用途：设置后续语音播放所使用的音量。

参数说明：

- `volume:number`：目标音量。小于 `0` 会钳制为 `0`，大于 `1` 会钳制为 `1`。

这个回调在实际使用中应当被当作纯 setter，看作没有实用返回值即可。

示例：

```lua
speechBox.setVolume(0.65)
```

## 说明

- 该组件依赖已安装的 TTS 后端。
- 当方块被卸载或移除时，正在播报的语音也会被自动停止。

## 相关内容

- `component`
- `speech`
