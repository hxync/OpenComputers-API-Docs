# server_destruct

## 概述

`server_destruct` 是 Computronics 提供的机架式自毁板组件。
它在 Lua 层暴露的倒计时接口形态与便携式 `self_destruct` 自毁卡相同，但它专门用于 OpenComputers 机架，并带有额外的机架维护逻辑。

## 可用性

- 仓库：`computronics`
- 组件名：`server_destruct`
- 常见宿主：安装在 OpenComputers 机架中的 Computronics 自毁板

## 用法

```lua
local component = require("component")
local boom = component.server_destruct
```

## 机架行为

- 该板卡在机架中运行时会持续消耗维护能量。
- 如果无法支付维护能量，它会自行失活，并清除当前已经设置的引信。
- 武装状态会保存到 NBT，因此普通存档保存后仍可保留。
- 当相邻电脑发送 `computer.started` 或 `computer.stopped` 事件时，引信会被重置。

## 爆炸行为

与便携式 `self_destruct` 卡不同，机架版不会只炸毁单个宿主。
当引信归零时，它会从当前机架开始，对附近机架做广度优先搜索，然后分批触发链式爆炸。

实现中的关键细节：

- 会向外搜索附近相连机架，搜索上限是平方距离 `256`
- 爆炸不是同一 tick 一次性全部触发，而是按批次排队执行
- 每批会隔若干服务端 tick 逐步爆炸

因此，`server_destruct` 的破坏范围通常明显大于普通 `self_destruct`。

## 接口

### `start`

- 语法：`boom.start([time])`
- 返回：
  - 成功：`seconds`
  - 失败：`-1, reason`
  - 严重失败：回调报错
- 用途：启动机架自毁引信

参数说明：

- `time:number` 可选：希望设置的引信秒数，默认值为 `5`

行为说明：

- 如果引信已经处于武装状态，则返回 `-1, "fuse has already been set"`
- 如果 `time > 100000`，则抛出 `time may not be greater than 100000`
- 内部实际保存的是 tick 计时器，计算方式来自 `floor(time * 20)`

示例：

```lua
local seconds = assert(boom.start(10))
print("Fuse:", seconds)
```

### `time`

- 语法：`boom.time()`
- 返回：
  - 已武装：`secondsRemaining`
  - 未武装：`-1, reason`
- 用途：返回当前剩余引信时间，单位为秒

行为说明：

- 若当前引信尚未设置，则返回 `-1, "fuse has not been set"`

示例：

```lua
local left, reason = boom.time()
print(left, reason)
```

## 与 `self_destruct` 的区别

- `self_destruct` 是普通宿主使用的便携式卡式组件。
- `server_destruct` 是安装在机架中的板卡组件。
- 二者都暴露 `start()` 和 `time()`。
- `server_destruct` 依赖机架维护能量，并且会沿附近机架传播爆炸。

## 相关内容

- `self_destruct`
- `particle`
