# self_destruct

## 概述

`self_destruct` 组件是 Computronics 的自毁卡。它可以启动一个倒计时引信，并在倒计时归零时在宿主位置触发爆炸，除非宿主电脑在此之前启动或停止从而把引信重置掉。

## 可用性

- 仓库：`computronics`
- 组件名：`self_destruct`
- 常见宿主：安装了 Computronics 自毁卡的兼容 OC 设备

## 用法

```lua
local component = require("component")
local boom = component.self_destruct
```

## 引信行为

- 引信内部以 tick 为单位保存。
- `start(seconds)` 会把秒数用 `floor(seconds * 20)` 转换成 tick。
- 一旦进入武装状态，引信会在每个服务端 tick 递减。
- 当它减到 `0` 时，卡片会在宿主位置调用 `SelfDestruct.goBoom(...)`。
- 引信状态会保存到 NBT，因此普通存档保存后仍会保留。
- 如果相邻电脑发送了 `computer.started` 或 `computer.stopped`，引信会被重置为 `-1`。

## 接口

### start

- 语法：`boom.start([time])`
- 返回：
  - 成功：`seconds`
  - 失败：`-1, reason`
  - 严重失败：抛出异常
- 用途：启动自毁引信。

参数说明：

- `time:number` 可选：希望设置的引信秒数。默认值为 `5`。

行为说明：

- 如果引信已经处于武装状态，则返回 `-1, "fuse has already been set"`。
- 如果时间大于 `100000`，则会抛出 `time may not be greater than 100000`。

示例：

```lua
local seconds = assert(boom.start(10))
print("Fuse:", seconds)
```

### time

- 语法：`boom.time()`
- 返回：
  - 已武装：`secondsRemaining`
  - 未武装：`-1, reason`
- 用途：返回当前剩余的引信时间，单位为秒。

行为说明：

- 如果引信尚未设置，则返回 `-1, "fuse has not been set"`。

示例：

```lua
local left, reason = boom.time()
print(left, reason)
```

## 相关内容

- `component`
- `particle`
