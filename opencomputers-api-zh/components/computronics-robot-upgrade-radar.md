# radar

## 概述

机器人上的 `radar` 组件用于扫描宿主周围的实体与掉落物。它适合做玩家探测、怪物预警、掉落物拾取逻辑以及基于距离的自动化控制。

扫描结果会以“列表风格”的 Lua 表返回，表中的每一项本身也是一个信息表。

## 可用性

- 仓库：`computronics`
- 组件名：`radar`
- 常见宿主：安装了 Computronics 雷达升级的机器人

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

- 默认最大扫描半径是 `8` 格。
- 不传参数时，会使用当前配置允许的最大半径。
- OpenComputers 下，这些扫描在真正执行时会让调用上下文暂停 `0.5` 秒。
- 扫描能耗会随请求距离和目标类型增长。

为了得到可预测的行为，建议把距离参数控制在 `1` 到 `8` 之间。实现虽然接受其他整数，但空间扫描范围仍然会被限制在配置允许的最大值内。

## 返回结构

这四个接口都会返回一个“列表风格”的表，其中每一项又是一个信息表。

实体与玩家条目字段：

- `name:string`
- `distance:number`
- `x:number`、`y:number`、`z:number`
  仅当配置没有强制启用“只返回距离”模式时才会出现

掉落物条目字段：

- `name:string`：物品注册名
- `label:string`：显示名称
- `size:number`：堆叠数量
- `damage:number`：损伤值或元数据
- `hasTag:boolean`
- `distance:number`
- `x:number`、`y:number`、`z:number`
  仅当不是“只返回距离”模式时才会出现

## 接口

### getEntities

- 语法：`radar.getEntities([distance])`
- 返回值：`table`
- 用途：返回范围内探测到的玩家与生物实体。

能耗行为：

- 消耗按 `distance * 1.75 * radarEnergyCost` 计算。

如果组件当前能量不足，这个接口不会报错，而是直接返回空表。

示例：

```lua
for _, entry in ipairs(radar.getEntities()) do
  print(entry.name, entry.distance)
end
```

### getPlayers

- 语法：`radar.getPlayers([distance])`
- 返回值：`table`
- 用途：仅返回探测到的玩家。

能耗行为：

- 消耗按 `distance * 1.0 * radarEnergyCost` 计算。

如果组件当前能量不足，这个接口会返回空表。

示例：

```lua
local players = radar.getPlayers(4)
for _, player in ipairs(players) do
  print("玩家:", player.name, player.distance)
end
```

### getMobs

- 语法：`radar.getMobs([distance])`
- 返回值：`table`
- 用途：仅返回探测到的生物实体。

能耗行为：

- 消耗按 `distance * 1.0 * radarEnergyCost` 计算。

如果组件当前能量不足，这个接口会返回空表。

示例：

```lua
local mobs = radar.getMobs()
print("生物数量:", #mobs)
```

### getItems

- 语法：`radar.getItems([distance])`
- 返回值：`table`
- 用途：返回探测到的掉落物实体。

能耗行为：

- 消耗按 `distance * 2.0 * radarEnergyCost` 计算。

如果组件当前能量不足，这个接口会返回空表。

示例：

```lua
for _, item in ipairs(radar.getItems(3)) do
  print(item.label, item.size, item.distance)
end
```

## 示例

```lua
local hostilesNearby = false

for _, mob in ipairs(radar.getMobs(5)) do
  if mob.distance < 3 then
    hostilesNearby = true
    print("附近生物:", mob.name)
  end
end

if hostilesNearby then
  print("近距离警报")
end
```

## 说明

- 机器人版本返回的是列表风格的 Lua 数组结果。
- 实体坐标字段只会在服务器配置允许返回距离以外信息时出现。

## 相关内容

- `component`
- `camera`
