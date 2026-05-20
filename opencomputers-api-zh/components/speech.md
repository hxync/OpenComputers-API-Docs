# speech

## 概述

`speech` 是 Computronics 提供的文本转语音组件。它可以让设备直接通过本地音频系统朗读文本，而不需要手工编排音调。

这个组件适合做语音通知、无障碍辅助、对话系统以及带语音播报的状态提示。

## 可用性

- 仓库：`computronics`
- 组件名：`speech`
- 常见宿主：安装了 Computronics 语音升级的机器人或其他设备

## 用法

```lua
local component = require("component")
local speech = component.speech

if not speech then
  error("未安装 speech 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### say

- 语法：`speech.say(text)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：开始朗读指定文本。

参数说明：

- `text:string`：要转换为语音并播放的文本。

行为说明：

- 设备同一时间只能处理一段文本。
- 如果当前正在播报，会返回 `false, "already processing"`。
- 文本长度默认不能超过 `300` 个字符。
- 如果当前环境没有可用的 TTS 后端，会返回 `false, "text-to-speech system not available"`。

常见失败返回：

- `false, "already processing"`
- `false, "text too long"`
- `false, "text-to-speech system not available"`

示例：

```lua
local ok, reason = speech.say("Reactor temperature is stable.")
if not ok then
  io.stderr:write("语音播报失败: " .. tostring(reason) .. "\n")
end
```

### stop

- 语法：`speech.stop()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：停止当前正在朗读的语句。

如果设备当前空闲，会返回 `false, "not talking"`。

示例：

```lua
local ok, reason = speech.stop()
if not ok and reason ~= "not talking" then
  io.stderr:write("停止播报失败: " .. tostring(reason) .. "\n")
end
```

### isProcessing

- 语法：`speech.isProcessing()`
- 返回值：`boolean`
- 用途：返回设备当前是否正在说话，或者仍在处理语音数据。

示例：

```lua
while speech.isProcessing() do
  os.sleep(0.1)
end
```

### setVolume

- 语法：`speech.setVolume(volume)`
- 返回值：无
- 用途：设置语音盒子的播放音量。

参数说明：

- `volume:number`：目标音量。小于 `0` 会被钳制到 `0`，大于 `1` 会被钳制到 `1`。

示例：

```lua
speech.setVolume(0.8)
```

## 说明

- 该组件依赖已安装的文本转语音后端。如果后端不可用，`say` 无法启动播报。
- 这个接口输出的是生成后的语音音频，而不是底层音素控制。

## 相关内容

- `component`
- `sound`
