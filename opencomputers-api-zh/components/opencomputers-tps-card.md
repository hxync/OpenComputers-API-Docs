# opencomputers-tps-card

## 概述

本页说明 `tps_card` 组件。它用于读取服务器和各维度的性能统计信息，例如平均 Tick 耗时、已加载实体数量、区块数量，以及按维度拆分的详细数据。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`tps_card`
- 对应实现：`li.cil.oc.server.component.TpsCard`
- 常见宿主：安装了 TPS 卡升级的宿主

## 可读取的数据

- 服务器整体平均 Tick 耗时，单位为毫秒。
- 每个已加载维度的平均 Tick 耗时。
- 当前已加载的维度、区块、方块实体、普通实体总数。
- 指定维度内按类名统计的实体和方块实体数量。
- 在权限允许时，读取某个实体类在指定维度中的所有坐标。

## 维度处理规则

部分接口只能查询“当前已加载”的维度。

- 如果目标维度未加载，这些接口会返回 `nil, "Dimension not loaded: <id>"`。
- `getTickTimeInDim()` 在省略参数时，会默认查询当前宿主所在维度。

## 类名规则

涉及实体列表和搜索的接口使用完整 Java 类名，例如：

- `net.minecraft.entity.item.EntityItem`
- `net.minecraft.entity.monster.EntityZombie`

如果你不确定准确类名，建议先调用 `getEntitiesListForDim()` 查看该维度里实际出现的类名。

## API

### getTickTimeInDim

- 语法：`tpsCard.getTickTimeInDim([dimension])`
- 参数：
  - `dimension:number`：维度 id，省略时默认使用宿主当前所在维度。
- 返回：`number`
- 用途：返回指定维度的平均 Tick 耗时，单位为毫秒。

数值越低越好。大约 `50` 毫秒对应 `20 TPS`。

### getOverallTickTime

- 语法：`tpsCard.getOverallTickTime()`
- 返回：`number`
- 用途：返回服务器整体平均 Tick 耗时，单位为毫秒。

### getAllDims

- 语法：`tpsCard.getAllDims()`
- 返回：`table`
- 用途：返回一个表，把每个已加载维度的 id 映射到维度名称。

### getAllTickTimes

- 语法：`tpsCard.getAllTickTimes()`
- 返回：`table`
- 用途：返回一个表，把每个已加载维度的 id 映射到该维度的平均 Tick 耗时，单位为毫秒。

### getNameForDim

- 语法：`tpsCard.getNameForDim(dimension)`
- 参数：
  - `dimension:number`：维度 id。
- 返回：
  - 成功时：`string`
  - 失败时：`nil, "Dimension not loaded: <id>"`
- 用途：返回指定已加载维度的显示名称。

### convertTickTimeIntoTps

- 语法：`tpsCard.convertTickTimeIntoTps(time)`
- 参数：
  - `time:number`：Tick 耗时，单位为毫秒。
- 返回：`number`
- 用途：把毫秒级 Tick 耗时转换为 TPS 数值。

行为细节：

- 结果上限为 `20`。
- 因此，小于 `50` 毫秒的耗时也最多只会显示 `20`。
- 这个辅助函数不会检查输入是否为正数。

### getOverallTileEntitiesLoaded

- 语法：`tpsCard.getOverallTileEntitiesLoaded()`
- 返回：`number`
- 用途：返回所有已加载维度中方块实体总数。

### getOverallChunksLoaded

- 语法：`tpsCard.getOverallChunksLoaded()`
- 返回：`number`
- 用途：返回所有已加载维度中区块总数。

### getOverallEntitiesLoaded

- 语法：`tpsCard.getOverallEntitiesLoaded()`
- 返回：`number`
- 用途：返回所有已加载维度中普通实体总数。

### getOverallDimsLoaded

- 语法：`tpsCard.getOverallDimsLoaded()`
- 返回：`number`
- 用途：返回当前服务器已加载的维度数量。

### getEntitiesListForDim

- 语法：`tpsCard.getEntitiesListForDim(dimension)`
- 参数：
  - `dimension:number`：维度 id。
- 返回：
  - 成功时：`table`
  - 失败时：`nil, "Dimension not loaded: <id>"`
- 用途：返回一个表，把该维度中的完整实体类名映射到对应已加载数量。

### getTileEntitiesListForDim

- 语法：`tpsCard.getTileEntitiesListForDim(dimension)`
- 参数：
  - `dimension:number`：维度 id。
- 返回：
  - 成功时：`table`
  - 失败时：`nil, "Dimension not loaded: <id>"`
- 用途：返回一个表，把该维度中的完整方块实体类名映射到对应已加载数量。

### getChunksLoadedForDim

- 语法：`tpsCard.getChunksLoadedForDim(dimension)`
- 参数：
  - `dimension:number`：维度 id。
- 返回：
  - 成功时：`number`
  - 失败时：`nil, "Dimension not loaded: <id>"`
- 用途：返回指定维度当前已加载的区块数量。

### getCoordinatesForEntityClassInDim

- 语法：`tpsCard.getCoordinatesForEntityClassInDim(className, dimension)`
- 参数：
  - `className:string`：完整 Java 实体类名。
  - `dimension:number`：维度 id。
- 返回：
  - 成功时：`table`
  - 权限不足时：`nil, "Access denied"`
  - 维度未加载时：`nil, "Dimension not loaded: <id>"`
- 用途：返回目标维度中所有运行时类精确等于 `className` 的已加载实体坐标。

返回结构：

- 返回表中的每一项对应一个匹配实体。
- 每一项都是 `{x, y, z}` 形式的三元坐标表。

## 示例

```lua
local component = require("component")

local tpsCard = component.tps_card
local ms = tpsCard.getOverallTickTime()

print("整体 Tick 耗时（ms）：", ms)
print("整体 TPS：", tpsCard.convertTickTimeIntoTps(ms))
print("已加载维度数：", tpsCard.getOverallDimsLoaded())
print("已加载区块数：", tpsCard.getOverallChunksLoaded())

local dims = tpsCard.getAllDims()
for id, name in pairs(dims) do
  print("维度", id, name, tpsCard.getTickTimeInDim(id))
end
```

## 相关内容

- `debug`
- `computer`
