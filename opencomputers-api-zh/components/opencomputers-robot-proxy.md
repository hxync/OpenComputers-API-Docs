# opencomputers-robot-proxy

## 概述

本页说明“已放置机器人方块”对外暴露的 `robot` 组件接口。相邻电脑可以通过这些接口读取机器人名称和运行状态，也可以启动、停止机器人，或者在关机状态下修改其名称。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`robot`
- 对应实现：`li.cil.oc.common.tileentity.RobotProxy`
- 常见宿主：从外部观察到的已放置机器人方块

## 它与 `robot` 页的区别

[`robot`](./robot.md) 页面记录的是“运行在机器人自身内部程序”可调用的接口。

而本页记录的是外部电脑通过组件扫描看到的代理视图，主要只能做这些事情：

- 启动机器人
- 停止机器人
- 查看是否正在运行
- 读取或修改名字

它不会暴露机器人内部的移动、物品栏、工具操作等机载接口。

## API

### start

- 语法：`robotProxy.start()`
- 返回：`boolean`
- 用途：尝试启动机器人，并返回运行状态是否发生变化。

行为说明：

- 如果机器人成功启动，返回 `true`。
- 如果机器人本来就在运行，或者处于无法这样启动的暂停状态，返回 `false`。

### stop

- 语法：`robotProxy.stop()`
- 返回：`boolean`
- 用途：尝试停止机器人，并返回运行状态是否发生变化。

如果机器人本来就已经停止，则返回 `false`。

### isRunning

- 语法：`robotProxy.isRunning()`
- 返回：`boolean`
- 用途：返回机器人机器实例当前是否正在运行。

### getName

- 语法：`robotProxy.getName()`
- 返回：`string`
- 用途：返回机器人当前名称。

### setName

- 语法：`robotProxy.setName(name)`
- 参数：
  - `name:string`：新的机器人名称。
- 返回：
  - 成功时：`string`
  - 失败时：`nil, "is running"`
- 用途：修改机器人名称，并返回旧名称。

限制：

- 只有在机器人未运行时才能修改名称。

## 示例

```lua
local component = require("component")

for address in component.list("robot") do
  local proxy = component.proxy(address)
  print(address, proxy.getName(), proxy.isRunning())
end
```

## 相关内容

- `robot`
- `computer`
