# printer3d

## 概述

`printer3d` 组件用于配置并启动 OpenComputers 3D 打印任务。它允许你使用多个长方体形状构建打印模型，设置标签和提示文本，配置发光与红石行为，决定碰撞规则，并把当前模型提交为可重复打印的模板。

## 可用性

- 仓库：`opencomputers`
- 组件名：`printer3d`
- 常见宿主：放置在世界中的 OpenComputers 3D 打印机

## 用法

```lua
local component = require("component")
local printer = component.printer3d

if not printer then
  error("未安装 printer3d 组件")
end
```

## 打印模型基础

- 一个模型分成两组形状：
  - `stateOff`
  - `stateOn`
- 只有当 `stateOff` 至少包含一个形状时，这个模型才可打印。
- `stateOn` 是可选的，用于表示激活状态下的附加模型。
- 每个形状都是一个长方体。
- 形状坐标会先被限制在 `0` 到 `16` 的体素范围内，再转换成方块内部的归一化坐标。

## 复杂度与限制

- 每个状态可容纳的最大形状数由 `Settings.get.maxPrintComplexity` 限制。
- `getMaxShapeCount()` 会返回该配置值。
- 标签会截断到 `24` 个字符。
- 提示文本会截断到 `128` 个字符。
- 发光等级会限制在 `0` 到 `Settings.get.maxPrintLightLevel` 之间。
- 红石强度会限制在 `0` 到 `15` 之间。
- `commit(count)` 会把 `count` 限制在 `0` 到 `Integer.MAX_VALUE` 之间。

## 材料与墨水

3D 打印机会消耗：

- 由变色龙材料提供的打印材料
- 打印墨水
- 来自 OC 网络缓冲区的能量

模型成本取决于：

- 所有形状总体积，对应材料消耗
- 所有形状总表面积，对应墨水消耗
- 使用自定义红石强度时额外增加的材料成本
- 任一状态设置为无碰撞时应用的 noclip 倍率

## 接口

### reset

- 语法：`printer.reset()`
- 返回：无可依赖的有效返回值
- 用途：清空当前模型配置，并停止继续排队新的打印任务。

行为说明：

- 已经开始的实体打印过程会继续完成。
- 由于源码会创建一个新的 `PrintData` 对象，因此形状、标签、提示、光照、红石和碰撞设置都会被重置。

### setLabel

- 语法：`printer.setLabel(value)`
- 返回：无可依赖的有效返回值
- 用途：设置打印方块的标签。

参数说明：

- `value:string`：要设置的标签，超过 `24` 个字符会被截断。

行为说明：

- 空字符串会被转换成 `nil`。

### getLabel

- 语法：`printer.getLabel()`
- 返回：`string|nil`
- 用途：返回当前待打印模型的标签。

### setTooltip

- 语法：`printer.setTooltip(value)`
- 返回：无可依赖的有效返回值
- 用途：设置打印方块的提示文本。

参数说明：

- `value:string`：提示文本，超过 `128` 个字符会被截断。

行为说明：

- 空字符串会被转换成 `nil`。

### getTooltip

- 语法：`printer.getTooltip()`
- 返回：`string|nil`
- 用途：返回当前待打印模型的提示文本。

### setLightLevel

- 语法：`printer.setLightLevel(value)`
- 返回：无可依赖的有效返回值
- 用途：设置打印方块的发光等级。

参数说明：

- `value:number`：请求的亮度等级。实现会把它限制到配置允许的最大值以内。

### getLightLevel

- 语法：`printer.getLightLevel()`
- 返回：`number`
- 用途：返回当前配置的发光等级。

### setRedstoneEmitter

- 语法：`printer.setRedstoneEmitter(value)`
- 返回：无可依赖的有效返回值
- 用途：配置激活状态下的红石输出。

参数说明：

- `value:boolean|number`：
  - `true` 会把红石强度设为 `15`
  - `false` 会把红石强度设为 `0`
  - 数值则会被限制到 `0` 到 `15`

### isRedstoneEmitter

- 语法：`printer.isRedstoneEmitter()`
- 返回：`boolean, number`
- 用途：返回当前模型是否发出红石，以及对应的强度等级。

### setButtonMode

- 语法：`printer.setButtonMode(value)`
- 返回：无可依赖的有效返回值
- 用途：设置打印方块在进入激活状态后是否会自动回到关闭状态。

### isButtonMode

- 语法：`printer.isButtonMode()`
- 返回：`boolean`
- 用途：返回当前按钮模式开关。

### setCollidable

- 语法：`printer.setCollidable(collideOff, collideOn)`
- 返回：无可依赖的有效返回值
- 用途：设置打印方块在关闭状态和开启状态下是否具有碰撞体积。

参数说明：

- `collideOff:boolean`
- `collideOn:boolean`

行为说明：

- 源码内部会反向写入 `noclipOff` 和 `noclipOn`。

### isCollidable

- 语法：`printer.isCollidable()`
- 返回：`boolean, boolean`
- 用途：返回关闭状态和开启状态的碰撞开关。

### addShape

- 语法：`printer.addShape(minX, minY, minZ, maxX, maxY, maxZ, texture[, state=false][, tint])`
- 返回：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：向当前模型添加一个长方体形状。

参数说明：

- `minX`、`minY`、`minZ:number`：第一个角点，单位为体素
- `maxX`、`maxY`、`maxZ:number`：第二个角点，单位为体素
- `texture:string`：纹理名，超过 `64` 个字符会被截断
- `state:boolean` 可选：`false` 表示加入关闭状态形状集，`true` 表示加入开启状态形状集
- `tint:number` 可选：该形状的染色值

行为说明：

- 坐标会先限制在 `0` 到 `16`。
- `minZ` 和 `maxZ` 会在内部用 `16 - value` 做翻转。
- 如果某一维厚度为 `0`，则会抛出 `empty block`。
- 如果形状数量已经超过配置允许的最大复杂度，则返回 `nil, "model too complex"`。

示例：

```lua
assert(printer.addShape(0, 0, 0, 16, 2, 16, "opencomputers:block/print"))
```

### getShapeCount

- 语法：`printer.getShapeCount()`
- 返回：`offCount, onCount`
- 用途：返回当前关闭状态和开启状态中各自已有的形状数量。

### getMaxShapeCount

- 语法：`printer.getMaxShapeCount()`
- 返回：`number`
- 用途：返回每个状态可使用的最大形状数配置值。

### commit

- 语法：`printer.commit([count])`
- 返回：
  - 成功：`true`
  - 失败：`nil, "model invalid"`
- 用途：把当前模型标记为可打印，并开始按请求数量持续输出成品。

参数说明：

- `count:number` 可选：要排队打印的份数，默认值为 `1`。

行为说明：

- 模型必须至少包含一个 `stateOff` 形状。
- 两组形状数量都必须不超过配置的复杂度上限。
- 当 `count <= 0` 时，`isActive` 会保持为 `false`，因此不会继续排队打印。

### status

- 语法：`printer.status()`
- 返回：
  - 正在打印时：`"busy", progress`
  - 空闲且模型有效时：`"idle", true`
  - 空闲但模型无效时：`"idle", false`
- 用途：返回打印机状态，以及当前进度或模型有效性。

行为说明：

- `progress` 是 `0` 到 `100` 的百分比。

## 示例

```lua
printer.reset()
printer.setLabel("Status Block")
printer.setTooltip("Printed by OC")
printer.setLightLevel(8)
printer.setRedstoneEmitter(15)
printer.setButtonMode(true)
printer.setCollidable(true, true)

assert(printer.addShape(0, 0, 0, 16, 4, 16, "opencomputers:block/print"))
assert(printer.addShape(4, 4, 4, 12, 12, 12, "opencomputers:block/print", true, 0xFF0000))

print("Shapes:", printer.getShapeCount())
assert(printer.commit(1))
```

## 相关内容

- `component`
- `opencomputers-openos-print3d`
