# thaumcraft

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Thaumcraft 集成接口。可用组件覆盖奥术合成机器人升级、注魔矩阵以及 aspect 容器。

## 可用性

- 依赖: `thaumcraft`
- 标签: `integration-required`

## 组件名

- `arcane_crafting`
- `infusion_matrix`
- `aspect_container`

## 主要用途

- 让机器人通过 Thaumcraft 升级执行原版或奥术合成。
- 监控注魔矩阵的激活状态、不稳定度和对称性。
- 读取罐子等 aspect 容器中的已存储要素。

## 组件

### arcane_crafting

`arcane_crafting` 由 Thaumcraft 奥术合成机器人升级暴露。

#### `craft([count])`

- 语法: `craft([count: number]): boolean, number[, string]`
- 用途: 尝试使用机器人左上角 3x3 合成区域进行合成，先匹配原版配方，不成功时再尝试 Thaumcraft 奥术配方。
- 参数:
  - `count: number`
    可选目标产物数量，会被限制在 `0` 到 `64` 之间。
- 返回值:
  - 是否成功至少合成出一部分。
  - 实际合成出的物品数量。
  - 失败时可能附带错误信息。
- 说明:
  - 如果普通合成已经成功，就不会再尝试奥术合成。
  - 常见失败信息包括:
    - `Can't find the recipe.`
    - `Missing a valid Wand to craft.`
    - `Insufficient Vis in wand to perform crafting.`

#### `craftNormal([count])`

- 语法: `craftNormal([count: number]): boolean, number[, string]`
- 用途: 只使用原版合成配方尝试合成物品。
- 参数:
  - `count: number`
    可选目标产物数量，范围会被限制在 `0` 到 `64`。
- 返回值:
  - 是否成功至少合成出一部分。
  - 实际合成出的物品数量。
  - 失败时可能附带错误信息。

#### `craftArcane([count])`

- 语法: `craftArcane([count: number]): boolean, number[, string]`
- 用途: 只使用 Thaumcraft 奥术合成配方尝试合成物品。
- 参数:
  - `count: number`
    可选目标产物数量，范围会被限制在 `0` 到 `64`。
- 返回值:
  - 是否成功至少合成出一部分。
  - 实际合成出的物品数量。
  - 失败时可能附带错误信息。
- 说明:
  - 机器人升级槽 `0` 中必须放有有效法杖。
  - 法杖必须有足够的 Vis 才能执行目标配方。
  - 机器人所有者还必须满足 Thaumcraft 一侧的研究和配方条件。

### infusion_matrix

`infusion_matrix` 用于 Thaumcraft 注魔矩阵。

#### `isActive()`

- 语法: `isActive(): boolean`
- 用途: 返回注魔矩阵当前是否处于激活状态。
- 返回值:
  - `true` 表示已激活。
  - `false` 表示未激活。

#### `isCrafting()`

- 语法: `isCrafting(): boolean`
- 用途: 返回注魔矩阵当前是否正在执行注魔合成。
- 返回值:
  - `true` 表示正在注魔。
  - `false` 表示当前没有进行中的注魔。

#### `getInstability()`

- 语法: `getInstability(): number`
- 用途: 返回当前不稳定度数值。
- 返回值:
  - 当前不稳定度。

#### `getSymmetry()`

- 语法: `getSymmetry(): number`
- 用途: 返回当前对称性数值。
- 返回值:
  - 当前对称性得分。

### aspect_container

`aspect_container` 用于实现 `IAspectContainer` 的 Thaumcraft 方块。

#### `getAspects()`

- 语法: `getAspects(): table`
- 用途: 返回当前方块中存储的全部 aspect。
- 返回值:
  - 由多个 aspect 表组成的数组。
- 说明:
  - 每个 aspect 表都包含:
    - `name`
    - `amount`

#### `getAspectCount(aspect)`

- 语法: `getAspectCount(aspect: string): number`
- 用途: 返回某一种指定 aspect 的存储数量。
- 参数:
  - `aspect: string`
    aspect 名称。调用前会先转成小写再匹配。
- 返回值:
  - 该 aspect 的当前存量。
- 说明:
  - 非法 aspect 名称会触发参数错误。

## 示例

```lua
local component = require("component")

if component.isAvailable("infusion_matrix") then
  local matrix = component.infusion_matrix
  print("Active:", matrix.isActive())
  print("Instability:", matrix.getInstability())
end

if component.isAvailable("aspect_container") then
  local jar = component.aspect_container
  for _, aspect in ipairs(jar.getAspects()) do
    print(aspect.name, aspect.amount)
  end
end
```

## 相关内容

- `thaumicenergistics`
- `bloodmagic`
