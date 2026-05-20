# beep

## 概述

`beep` 是 Computronics 提供的简易多音蜂鸣器组件，适合播放提示音、短旋律和状态反馈音，不需要像 `sound` 声卡那样维护完整的指令队列。

该组件最多可以同时播放 8 个音调。每个音调都是方波，并且可以分别指定频率与持续时间。

## 可用性

- 仓库：`computronics`
- 组件名：`beep`
- 常见宿主：可安装 Computronics 蜂鸣卡的 OpenComputers 设备

## 用法

```lua
local component = require("component")
local beep = component.beep

if not beep then
  error("未安装 beep 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### beep

- 语法：`beep.beep(frequencyDurationTable)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：立即播放一个或多个音调。

参数说明：

- `frequencyDurationTable:table`：Lua 表，键为频率（Hz），值为持续时间（秒）。

行为说明：

- 表中最多只能包含 `8` 个频率项。
- 每个频率都必须是数字，并且范围必须在 `20` 到 `2000` Hz 之间。
- 持续时间如果提供，必须是数字。
- 持续时间省略或为 `nil` 时，默认按 `0.1` 秒处理。
- 持续时间会被限制在最短 `50` 毫秒、最长 `5000` 毫秒。
- 如果本次请求的音调数量加上当前正在播放的音调数量超过 `8`，调用会直接失败。

常见失败返回：

- `false, "table must not contain more than 8 frequencies"`
- `false, "already too many sounds playing, maximum is 8"`
- 组件网络能量不足时返回 `false, reason`

示例：

```lua
local ok, reason = beep.beep({
  [440] = 0.15,
  [660] = 0.15,
  [880] = 0.30
})

if not ok then
  io.stderr:write("播放蜂鸣音失败: " .. tostring(reason) .. "\n")
end
```

示例：成功提示音与错误提示音。

```lua
local function successTone()
  assert(beep.beep({[880] = 0.08, [1320] = 0.12}))
end

local function errorTone()
  assert(beep.beep({[220] = 0.20, [180] = 0.25}))
end
```

### getBeepCount

- 语法：`beep.getBeepCount()`
- 返回值：`number`
- 用途：返回当前正在占用的蜂鸣通道数量。

这个接口适合在脚本里判断设备是否仍然忙碌，避免过早继续塞入更多音调。

示例：

```lua
if beep.getBeepCount() == 0 then
  beep.beep({[523.25] = 0.1})
end
```

## 说明

- 该组件始终输出方波音色。
- 与 `sound` 声卡不同，它不提供指令队列、独立通道控制或调制功能。

## 相关内容

- `component`
- `sound`
- `noise`
