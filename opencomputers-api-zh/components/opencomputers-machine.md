# computer

## 概述

本页记录的是 OpenComputers 机器节点自带的基础组件。虽然生成出来的文件名是 `opencomputers-machine.md`，但这个节点在 Lua 中真正暴露的组件名是 `computer`。

它提供的是最基础的机器控制与自检接口：

- 启动和停止
- 运行状态查询
- 简单蜂鸣反馈
- 设备信息枚举
- 已知程序来源位置查询

## 可用性

- 仓库：`opencomputers`
- 实际组件名：`computer`
- 常见宿主：OpenComputers 电脑本体或机箱机器节点

## 用法

```lua
local component = require("component")
local computerComponent = component.computer

if not computerComponent then
  error("computer 组件不可用")
end
```

## 接口

### start

- 语法：`computerComponent.start()`
- 返回：`boolean`
- 用途：在机器当前处于停止状态时尝试启动它。

行为说明：

- 只有真正发生状态变化时才返回 `true`。
- 如果机器已经在运行、正在停止，或当前条件不允许启动，则返回 `false`。
- 启动过程中如果因为没有 CPU、没有内存、没有电力或组件超限等内部原因失败，机器会发出对应崩溃反馈，而该回调最终仍返回 `false`。

### stop

- 语法：`computerComponent.stop()`
- 返回：`boolean`
- 用途：请求关闭机器。

行为说明：

- 只有在本次调用真正新建了一个停止请求时才返回 `true`。
- 如果机器已经停止或已经处于停止流程中，则返回 `false`。

### isRunning

- 语法：`computerComponent.isRunning()`
- 返回：`boolean`
- 用途：返回当前机器是否被视为“正在运行”。

行为说明：

- 机器处于暂停状态时，这里仍然会返回 `true`。
- 只有当机器顶层状态是 `Stopped` 或 `Stopping` 时，它才会变成 `false`。

### beep

- 语法：
  - `computerComponent.beep(pattern)`
  - `computerComponent.beep([frequency[, duration]])`
- 返回：无可依赖的有效返回值
- 用途：播放声音反馈或蜂鸣模式串。

参数说明：

- `pattern:string`：OpenComputers 蜂鸣模式字符串。
- `frequency:number` 可选：音调频率，单位赫兹。
- `duration:number` 可选：持续时长，单位秒。

行为说明：

- 数值频率必须位于 `20` 到 `2000` 之间，否则会抛出 `invalid frequency, must be in [20, 2000]`。
- 数值时长会先换算成毫秒，再限制到 `50` 到 `5000` 之间。
- 对于数值蜂鸣，调用上下文会暂停对应的持续时长。

示例：

```lua
computerComponent.beep(880, 0.2)
computerComponent.beep("..")
```

### getDeviceInfo

- 语法：`computerComponent.getDeviceInfo()`
- 返回：`table`
- 用途：收集当前机器网络中所有可见或可达设备的元数据。

行为说明：

- 因为遍历整个节点网络可能比较昂贵，这个回调会暂停 `1` 秒。
- 返回表以组件地址作为键。
- 每个值都是一个设备信息表，通常会包含 class、description、vendor、product 等字段，前提是该宿主设备本身提供这些元数据。

### getProgramLocations

- 语法：`computerComponent.getProgramLocations()`
- 返回：`table`
- 用途：返回一个“程序名 -> 提供该程序的磁盘标签”的映射表。

行为说明：

- 该映射会依赖当前启用的体系结构名称。

## 示例

```lua
print("Running:", computerComponent.isRunning())

for address, info in pairs(computerComponent.getDeviceInfo()) do
  print(address, info.description, info.product)
end

for program, disk in pairs(computerComponent.getProgramLocations()) do
  print(program, disk)
end
```

## 相关内容

- `component`
- `opencomputers-screen`
