# opencomputers-upgrade-generator

## 概述

本页记录安装在机器人等移动宿主上的升级发电机所暴露出的 `generator` 组件接口。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`generator`
- 常见宿主：机器人、无人机，以及其他支持发电机升级的代理类宿主

## 作用

- 在内部维护一个排队等待燃烧的燃料堆。
- 随时间燃烧排队中的燃料，为组件缓冲补充能量。
- 允许 Lua 从当前选中槽位插入燃料，或把排队中的燃料取回库存。

## 队列规则

- 同一时间队列中只能存在一种燃料类型。
- 队列大小受该燃料物品自身最大堆叠数量限制。
- 某些带容器返还物的燃料，在插入或取出时可能要求库存里有额外空间。

## API

### insert

- 语法：`generator.insert([count])`
- 返回值：
  - 成功：`true, insertedCount`
  - 失败：`nil, reason` 或 `false, reason`
- 用途：把当前选中库存槽位中的燃料移动到发电机队列中。

可能失败原因：

- `selected slot is empty`
- `selected slot does not contain fuel`
- `different fuel type already queued`
- `queue is full`
- `no space in inventory for fuel containers`

`count` 默认是 `64`。

### count

- 语法：`generator.count()`
- 返回值：
  - 队列为空时：`0`
  - 队列非空时：`queuedCount, fuelDisplayName`
- 用途：报告当前队列中还有多少燃料。

### remove

- 语法：`generator.remove([count])`
- 返回值：
  - 成功：`true` 或 `true, removedCount`
  - 失败：`false, reason`
- 用途：把排队中的燃料从发电机中取出并返还到玩家库存。

可能失败原因：

- `queue is empty`
- `removing this fuel requires the appropriate container in the selected slot`
- `no inventory space available for fuel`

行为细节：

- 传入 `0` 或负数数量是允许的，会直接返回 `true`。
- 如果该燃料在取出时需要空容器，当前选中槽位中必须已经放着匹配的空容器，才能成功移除。

## 使用说明

- 如果组件断开连接时队列里还有燃料，剩余燃料会直接掉落到世界中。
- 当当前燃烧时间耗尽后，发电机会自动继续消耗队列中的燃料。
- 每 tick 产能多少由 OpenComputers 的 generator efficiency 配置控制。

## 示例

```lua
local component = require("component")
local robot = require("robot")
local generator = component.generator

robot.select(1)

local ok, value = generator.insert(4)
print("Insert result:", ok, value)

local queued, name = generator.count()
print("Queued fuel:", queued, name)

local removed, countOrReason = generator.remove(2)
print("Remove result:", removed, countOrReason)
```

## 相关内容

- `robot`
- `drone`
- `computer`
