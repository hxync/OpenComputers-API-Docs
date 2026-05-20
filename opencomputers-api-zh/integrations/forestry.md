# forestry

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Forestry 集成接口。可用组件覆盖分析台、养蜂设施以及机器人 `beekeeper` 升级。

## 可用性

- 依赖: `forestry`
- 标签: `integration-required`

## 组件名

- `forestry_analyzer`
- `bee_housing`
- `beekeeper`

## 主要用途

- 从 Forestry 分析台读取已分析蜜蜂数据。
- 检查蜂巢/蜂箱中的工蜂、蜂后和突变信息。
- 让机器人通过 `beekeeper` 升级交换蜜蜂、分析样本、管理 GregTech 工业蜂房升级。

## Forestry 个体返回表

本页有多个接口会返回 OpenComputers 转换后的 Forestry 个体数据表。

常见顶层字段包括:

- `displayName`
- `ident`
- `isAnalyzed`
- `isSecret`
- `type`

根据样本类型不同，还可能包含:

- `health`
- `maxHealth`
- `generation`
- `isAlive`
- `isNatural`
- `canSpawn`
- `hasEffect`
- `active`
- `inactive`

对蜜蜂来说，`active` 和 `inactive` 子表通常会包含这些染色体信息:

- `species`
- `speed`
- `lifespan`
- `fertility`
- `temperatureTolerance`
- `humidityTolerance`
- `nocturnal`
- `tolerantFlyer`
- `caveDwelling`
- `flowerProvider`
- `flowering`
- `effect`
- `territory`

## 组件

### forestry_analyzer

`forestry_analyzer` 用于 Forestry 分析台。

#### `isWorking()`

- 语法: `isWorking(): boolean`
- 用途: 返回分析台当前是否有可执行的分析工作。
- 返回值:
  - `true` 表示当前可以处理样本。
  - `false` 表示当前不能工作。

#### `getProgress()`

- 语法: `getProgress(): number`
- 用途: 返回当前分析操作进度。
- 返回值:
  - `0` 到 `1` 之间的进度值。
- 说明:
  - 驱动把 Forestry 的缩放进度转换成了标准化比例，值越大表示越接近完成。
  - 这是一个归一化后的浮点比例，不是原版界面上直接显示的整数百分比。

#### `getIndividualOnDisplay()`

- 语法: `getIndividualOnDisplay(): table`
- 用途: 返回分析台当前显示的个体数据。
- 返回值:
  - Forestry 个体数据表。
- 说明:
  - 返回表结构取决于当前显示的是哪一种 Forestry 样本。

### bee_housing

`bee_housing` 用于 Forestry 的养蜂设施，例如 apiary、alveary 以及其他兼容蜂类住房。

#### `canBreed()`

- 语法: `canBreed(): boolean`
- 用途: 返回当前养蜂设施是否能正常工作并繁殖蜜蜂。
- 返回值:
  - `true` 表示养蜂逻辑允许工作。
  - `false` 表示当前不能工作。

#### `getDrone()`

- 语法: `getDrone(): table`
- 用途: 返回当前设施中安装的工蜂数据。
- 返回值:
  - 工蜂的 Forestry 个体数据表。
  - 如果当前没有工蜂，则返回 `nil`。

#### `getQueen()`

- 语法: `getQueen(): table`
- 用途: 返回当前设施中安装的蜂后数据。
- 返回值:
  - 蜂后的 Forestry 个体数据表。
  - 如果当前没有蜂后，则返回 `nil`。

#### `getBeeBreedingData()`

- 语法: `getBeeBreedingData(): table`
- 用途: 返回 Forestry 蜜蜂系统中已注册的突变定义。
- 返回值:
  - 由多个突变描述表组成的集合式结果。
- 说明:
  - 每个突变表通常包含:
    - `allele1`
    - `allele2`
    - `chance`
    - `specialConditions`
    - `result`
  - 适合用来做游戏内突变查询或配方辅助工具。

#### `listAllSpecies()`

- 语法: `listAllSpecies(): table`
- 用途: 返回所有已知的蜜蜂物种输出。
- 返回值:
  - 物种描述表集合。
- 说明:
  - 每个物种条目通常带有 `name`、`uid`、`humidity`、`temperature` 等字段。

#### `getBeeParents(beeName)`

- 语法: `getBeeParents(beeName: string): table`
- 用途: 返回能够生成指定蜜蜂物种的突变定义。
- 参数:
  - `beeName: string`
    物种本地化名称或 UID，不区分大小写。
- 返回值:
  - 匹配到的突变记录集合。
- 说明:
  - 匹配时会同时检查物种本地化名称和 UID。

### beekeeper

`beekeeper` 由 Forestry 养蜂机器人升级暴露。

#### `swapQueen(side)`

- 语法: `swapQueen(side: number): boolean`
- 用途: 把机器人当前选中槽位与指定方向蜂类住房中的蜂后槽进行交换。
- 参数:
  - `side: number`
    目标养蜂设施相对机器人的方向。
- 返回值:
  - 找到兼容养蜂设施并完成交换时返回 `true`。
  - 否则返回 `false`。

#### `swapDrone(side)`

- 语法: `swapDrone(side: number): boolean`
- 用途: 把机器人当前选中槽位与指定方向蜂类住房中的工蜂槽进行交换。
- 参数:
  - `side: number`
    目标养蜂设施相对机器人的方向。
- 返回值:
  - 找到兼容养蜂设施并完成交换时返回 `true`。
  - 否则返回 `false`。

#### `getBeeProgress(side)`

- 语法: `getBeeProgress(side: number): number[, string]`
- 用途: 返回指定方向养蜂设施当前蜜蜂工作进度百分比。
- 参数:
  - `side: number`
    目标养蜂设施相对机器人的方向。
- 返回值:
  - 成功时返回当前进度百分比。
  - 如果没有找到兼容养蜂设施，则返回 `false, "No bee housing found"`。
- 说明:
  - 这个值来自 Forestry 的蜂类工作进度百分比接口，通常可以直接按百分比理解。

#### `canWork(side)`

- 语法: `canWork(side: number): boolean[, string]`
- 用途: 返回指定方向养蜂设施当前是否可以工作。
- 参数:
  - `side: number`
    目标养蜂设施相对机器人的方向。
- 返回值:
  - 成功时返回 `true` 或 `false`。
  - 如果没有找到兼容养蜂设施，则返回 `false, "No bee housing found"`。

#### `analyze(honeySlot)`

- 语法: `analyze(honeySlot: number): boolean[, string]`
- 用途: 分析机器人当前选中槽中的蜜蜂样本，并消耗指定槽位中的一个蜂蜜物品。
- 参数:
  - `honeySlot: number`
    机器人库存中放有 `honeydew` 或 `honeyDrop` 的槽位。
- 返回值:
  - 成功时返回 `true`。
  - 如果当前选中槽不是蜜蜂，则返回 `false, "Not a bee"`。
  - 如果指定蜂蜜槽没有有效蜂蜜物品，则返回 `false, "No honey!"`。
- 说明:
  - 被分析的样本始终来自机器人当前选中的库存槽位。

#### `addIndustrialUpgrade(side[, amount])`

- 语法: `addIndustrialUpgrade(side: number[, amount: number]): number`
- 用途: 把机器人当前选中槽中的工业蜂房升级安装到指定方向的 GregTech 工业蜂房中。
- 参数:
  - `side: number`
    目标工业蜂房相对机器人的方向。
  - `amount: number`
    可选，本次尝试安装的升级数量，默认是 `64`。
- 返回值:
  - 成功安装的升级数量。
- 说明:
  - 如果没找到 GregTech 工业蜂房、当前选中槽为空，或者没有任何升级能安装进去，则返回 `0`。

#### `getIndustrialUpgrade(side, slot)`

- 语法: `getIndustrialUpgrade(side: number, slot: number): table[, string]`
- 用途: 返回指定方向 GregTech 工业蜂房某个升级槽中的物品。
- 参数:
  - `side: number`
    目标工业蜂房相对机器人的方向。
  - `slot: number`
    从 `1` 开始的工业升级槽编号。
- 返回值:
  - 该槽位中的升级物品栈描述表。
  - 如果槽位编号非法，则返回 `nil, "Wrong slot index (should be 1-X)"`。
- 说明:
  - 这里的 `X` 会在实际返回时替换成当前版本支持的最大工业升级槽编号。

#### `removeIndustrialUpgrade(side, slot[, amount])`

- 语法: `removeIndustrialUpgrade(side: number, slot: number[, amount: number]): boolean|number[, string]`
- 用途: 从 GregTech 工业蜂房中移除升级，并尝试放回机器人库存。
- 参数:
  - `side: number`
    目标工业蜂房相对机器人的方向。
  - `slot: number`
    从 `1` 开始的工业升级槽编号。
  - `amount: number`
    可选，要移除的升级数量，默认是 `64`。
- 返回值:
  - 成功移回机器人库存的升级数量。
  - 如果槽位编号非法，则返回 `false, "Wrong slot index (should be 1-X)"`。
- 说明:
  - 这里的 `X` 会在实际返回时替换成当前版本支持的最大工业升级槽编号。
  - 工具函数会优先尝试放回机器人当前选中槽，其次合并到相同物品堆，最后再找任意空槽。

## 示例

```lua
local component = require("component")

if component.isAvailable("bee_housing") then
  local housing = component.bee_housing
  local queen = housing.getQueen()
  if queen then
    print("Queen:", queen.displayName, queen.isAnalyzed)
  end
end

if component.isAvailable("beekeeper") then
  local keeper = component.beekeeper
  print("Can work:", keeper.canWork(3))
end
```

## 相关内容

- `gregtech`
- `vanilla`
