# opencomputers-redstone-vanilla

## 概述

本页说明标准 `redstone` 组件中用于普通模拟红石输入输出的接口。这些接口常见于红石卡、适配器、中继器、交换机、具备红石能力的微控制器，以及其他暴露 OpenComputers 红石控制能力的宿主。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`redstone`
- 底层特征：`RedstoneVanilla`

## 用途

- 读取单个面或全部六个面的模拟红石输入。
- 读取组件当前维护的模拟红石输出值。
- 设置单个面或多个面的模拟红石输出。
- 读取相邻方块提供的比较器输出强度。

侧面编号使用 OpenComputers 常见的 `0` 到 `5` 方向常量，实际脚本里通常配合 `sides` 库使用。

## 事件

当某一侧的红石输入发生变化时，组件会发出事件：

```lua
"redstone_changed", side, oldValue, newValue
```

- `side`：发生变化的侧面编号。
- `oldValue`：变化前的模拟红石强度。
- `newValue`：变化后的模拟红石强度。

如果宿主配置了唤醒阈值，并且新值向上跨过该阈值，事件还可以用于唤醒休眠中的计算机。

## API

### getInput

- 语法：`redstone.getInput([side])`
- 参数：
  - `side:number`：可选，要查询的侧面编号。
- 返回值：
  - 传入 `side` 时：`number`
  - 不传参数时：`table`
- 用途：读取输入到组件中的模拟红石强度。
- 说明：
  - 不传参数时，返回值是一个以侧面编号为键的表。
  - 常见范围是 `0` 到 `15`。

### getOutput

- 语法：`redstone.getOutput([side])`
- 参数：
  - `side:number`：可选，要查询的侧面编号。
- 返回值：
  - 传入 `side` 时：`number`
  - 不传参数时：`table`
- 用途：读取组件当前已经设置的模拟红石输出值。
- 说明：
  - 不传参数时返回全部侧面的输出状态。

### setOutput

- 语法：
  - `redstone.setOutput(side, value)`
  - `redstone.setOutput(valuesTable)`
- 参数：
  - `side:number`：要设置的侧面编号。
  - `value:number`：新的输出强度，通常使用 `0` 到 `15`。
  - `valuesTable:table`：批量设置表，键为侧面编号，值为对应输出强度。
- 返回值：
  - 单侧设置形式：该侧修改前的旧输出值。
  - 表设置形式：全部侧面修改前的旧输出表。
- 用途：更新模拟红石输出。
- 说明：
  - 适合做开门、点灯、触发活塞、输出控制脉冲等操作。
  - 若启用了 OpenComputers 的红石延迟配置，调用后脚本可能会短暂停顿。

### getComparatorInput

- 语法：`redstone.getComparatorInput(side)`
- 参数：
  - `side:number`：要读取的侧面编号。
- 返回值：`number`
- 用途：读取该侧相邻方块向外提供的比较器输出强度。
- 说明：
  - 如果相邻方块不支持比较器输出，则返回 `0`。
  - 可用于检测箱子内容、熔炉状态或其他会影响比较器信号的方块。

## 使用说明

- 红石强度通常使用 Minecraft 标准范围 `0` 到 `15`。
- 侧面编号会先按照宿主自身朝向解释，再映射到世界中的实际方向。
- 批量设置适合一次性刷新多个面的输出状态，避免分散写入。

## 使用示例

```lua
local component = require("component")
local sides = require("sides")
local redstone = component.redstone

print("Front input:", redstone.getInput(sides.front))
print("All outputs:", redstone.getOutput())

local previous = redstone.setOutput(sides.back, 15)
print("Previous back output:", previous)
print("Comparator on top:", redstone.getComparatorInput(sides.top))
```

### 示例用途

这个示例会读取前方输入、查看全部输出状态、把后方输出拉满到 `15`，并读取上方方块的比较器强度。

## 相关内容

- `opencomputers-redstone-bundled`
- `opencomputers-redstone-wireless`
- `opencomputers-redstone-signaller`
