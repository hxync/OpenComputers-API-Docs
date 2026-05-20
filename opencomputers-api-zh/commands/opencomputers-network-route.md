# route

## 概述

显示内置软件网络栈维护的 MCNET 路由表。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\route.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\route`

## 语法

```lua
route
```

## 用途

把 `network.info.getRoutes()` 返回的所有已知路由条目整理成定宽表格输出。命令会先打印 `MCNET routing table` 标题，然后列出每个目标地址对应的网关路由器以及实际发送流量所使用的接口。

## 输出

- `Destination`: MCNET 路由表中的目标主机或目标网络键。
- `Gateway`: 到达该目标时要经过的路由器地址。
- `Iface`: 向该目标发送数据时使用的接口名称。

## 示例

```lua
route
```
显示当前所有已知的 MCNET 路由条目。

## 备注

- 各列宽度会根据当前最长的目标、网关和接口值动态计算。
- 路由条目的输出顺序取决于 Lua `pairs` 的遍历结果，因此显示顺序不保证固定。
