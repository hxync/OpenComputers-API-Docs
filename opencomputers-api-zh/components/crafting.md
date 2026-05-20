# crafting

## 概述

`crafting` 组件提供机器人合成升级的 Lua 接口。它允许机器人使用自身物品栏左上角映射出来的 `3 x 3` 合成区域执行工作台配方。

虽然这个组件只有一个核心接口，但它的行为细节很重要：它会反复尝试合成同一种配方，直到达到请求数量、材料不足，或者输出结果发生变化为止。

## 可用性

- 仓库：`opencomputers`
- 组件名：`crafting`
- 常见宿主：安装了合成升级的机器人

## 用法

```lua
local component = require("component")
local crafting = component.crafting

if not crafting then
  error("当前机器人未安装 crafting 合成升级")
end
```

## 物品栏映射

合成升级会把机器人物品栏左上角的区域解释为一个 `3 x 3` 合成矩阵：

```text
槽位 1  槽位 2  槽位 3
槽位 5  槽位 6  槽位 7
槽位 9  槽位 10 槽位 11
```

也就是说，机器人每一行 4 个槽位里，只有前 3 个槽位会参与合成。

## 接口

### craft

- 语法：`crafting.craft([count])`
- 返回值：`boolean, number`
- 用途：根据当前合成矩阵中的配方，重复执行合成。

参数说明：

- `count:number` 可选：期望产出的物品数量，默认值为 `64`。

行为说明：

- 请求数量会被限制在 `0` 到 `64` 之间。
- 组件会先识别当前合成区域中存在的配方。
- 之后只要满足以下条件，就会持续合成：
  - 材料仍然足够；
  - 当前配方仍然有效；
  - 合成产物仍然是同一种物品。
- 容器物品和合成副产物会尽量放回机器人物品栏。

返回值说明：

- 第一个返回值：存在有效配方时为 `true`，否则为 `false`。
- 第二个返回值：实际成功产出的物品总数。

示例：

尽可能合成，最多做到一整组：

```lua
local ok, crafted = crafting.craft()
if ok then
  print("合成出的物品数量:", crafted)
else
  print("当前合成区域中没有有效配方")
end
```

只尝试做出一份目标产物：

```lua
local ok, crafted = crafting.craft(1)
print("是否识别到配方:", ok)
print("实际产出数量:", crafted)
```

## 相关内容

- `inventory_controller`
- `robot`
- `component`
