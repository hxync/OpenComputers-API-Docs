# radar

## 概述

方块版 `radar` 组件用于扫描放置式雷达方块周围的玩家、生物和掉落物。它适合做区域监控、警报系统、掉落物追踪和固定式周界传感器。

和机器人版本一样，结果会以列表风格的表返回，每个条目本身仍是一个信息表。

## 可用性

- 仓库：`computronics`
- 组件名：`radar`
- 常见宿主：放置在世界中的 Computronics 雷达方块

## 用法

```lua
local component = require("component")
local radar = component.radar

if not radar then
  error("未安装 radar 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 扫描模型

- 默认最大扫描半径为 `8` 格。
- 不传参数时，会使用当前配置允许的最大半径。
- OpenComputers 下，成功执行的扫描会让上下文暂停 `0.5` 秒。
- 方块版雷达既可以从 OpenComputers 连接器缓冲取电，也可以从自身内部电池取电。

为了得到稳定结果，建议把距离参数控制在 `1` 到 `8` 之间。

## 返回结构

所有接口都会返回一个从 `1` 开始的列表风格表。

实体与玩家条目字段：

- `name:string`
- `distance:number`
- `x:number`、`y:number`、`z:number`
  仅在未启用“只返回距离”模式时出现

掉落物条目字段：

- `name:string`
- `label:string`
- `size:number`
- `damage:number`
- `hasTag:boolean`
- `distance:number`
- `x:number`、`y:number`、`z:number`
  仅在未启用“只返回距离”模式时出现

## 接口

### getEntities

- 语法：`radar.getEntities([distance])`
- 返回值：`table`
- 用途：返回探测到的玩家和生物实体。

能耗行为：

- 消耗按 `distance * 1.75 * radarEnergyCost` 计算。

如果 OpenComputers 缓冲和内部电池都无法支付这次扫描，接口会返回空表。

示例：

```lua
for _, entity in pairs(radar.getEntities()) do
  print(entity.name, entity.distance)
end
```

### getPlayers

- 语法：`radar.getPlayers([distance])`
- 返回值：`table`
- 用途：仅返回探测到的玩家。

能耗行为：

- 消耗按 `distance * 1.0 * radarEnergyCost` 计算。

如果无法提供扫描能量，接口返回空表。

示例：

```lua
local players = radar.getPlayers()
for _, player in pairs(players) do
  print(player.name)
end
```

### getMobs

- 语法：`radar.getMobs([distance])`
- 返回值：`table`
- 用途：仅返回探测到的生物实体。

能耗行为：

- 消耗按 `distance * 1.0 * radarEnergyCost` 计算。

如果无法提供扫描能量，接口返回空表。

示例：

```lua
local mobs = radar.getMobs(6)
print("生物条目数:", #mobs)
```

### getItems

- 语法：`radar.getItems([distance])`
- 返回值：`table`
- 用途：返回探测到的掉落物实体。

能耗行为：

- 消耗按 `distance * 2.0 * radarEnergyCost` 计算。

如果无法提供扫描能量，接口返回空表。

示例：

```lua
for _, item in pairs(radar.getItems(4)) do
  print(item.label, item.size)
end
```

## 说明

- 方块版雷达的返回结果来自 Java `Set` 转成的整数键表，因此可以把它当列表使用，但不要依赖扫描结果的固定顺序。
- 坐标字段只会在配置允许返回距离以外的信息时出现。

## 相关内容

- `component`
- `camera`
