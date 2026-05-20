# opencomputers-redstone-bundled

## 概述

本页记录高级 `redstone` 组件暴露出的捆绑红石扩展接口。捆绑红石在每个侧面上都携带 16 条独立颜色通道，只有在安装了受支持的红石集成模组时才可用。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`redstone`
- 底层实现特征：`RedstoneBundled`

## 作用

- 读取单色、单侧面或全部侧面的捆绑输入。
- 读取单色或单侧面的捆绑输出。
- 以“单颜色 / 单侧面 / 嵌套侧面表”的方式设置捆绑输出。
- 在捆绑输入变化时，发出带颜色索引的 `redstone_changed` 信号。

颜色索引范围是 `0` 到 `15`。

## 信号形式

捆绑输入变化时会发出：

```lua
"redstone_changed", side, oldValue, newValue, color
```

最后一个 `color` 参数表示发生变化的颜色通道编号。

## API

### getBundledInput

- 语法：
  - `redstone.getBundledInput()`
  - `redstone.getBundledInput(side)`
  - `redstone.getBundledInput(side, color)`
- 返回值：
  - 不传参数：`side -> colorTable` 的表
  - 传一个参数：`color -> value` 的表
  - 传两个参数：`number`
- 用途：读取捆绑红石输入值。

输入值会取该侧面所有受支持捆绑红石来源中的最大值。

### getBundledOutput

- 语法：
  - `redstone.getBundledOutput()`
  - `redstone.getBundledOutput(side)`
  - `redstone.getBundledOutput(side, color)`
- 返回值：
  - 不传参数：全部侧面的表状快照
  - 传一个参数：`color -> value` 的表
  - 传两个参数：`number`
- 用途：读取捆绑红石输出值。

版本说明：

- 在这版源码里，无参数形式走到的内部快照实现看起来存在不一致行为。为了获得精确结果，建议优先使用 `getBundledOutput(side)` 或 `getBundledOutput(side, color)`。

### setBundledOutput

- 语法：
  - `redstone.setBundledOutput(side, color, value)`
  - `redstone.setBundledOutput(side, colorTable)`
  - `redstone.setBundledOutput(sideTable)`
- 返回值：
  - 单颜色形式：该通道修改前的旧值
  - 单侧面表形式：该侧面修改前的旧值表
  - 全侧面表形式：修改前的整体快照
- 用途：更新捆绑红石输出。

可接受的表结构：

- `colorTable`：`颜色 -> 值`
- `sideTable`：`侧面 -> colorTable`

## 使用说明

- 每个捆绑颜色通道彼此独立，设置某一种颜色不会影响其它颜色。
- 和普通红石输出一样，如果启用了红石延迟，写入操作可能会导致脚本短暂停顿。
- 这个高级红石控制器在设备信息中会把宽度报告为 `16`，容量报告为 `65536`。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local redstone = component.redstone

local old = redstone.setBundledOutput(sides.left, 3, 255)
print("Old color 3 value on the left:", old)

local left = redstone.getBundledOutput(sides.left)
print("Left side color 3 now:", left[3])

local input = redstone.getBundledInput(sides.right, 5)
print("Right side bundled input color 5:", input)
```

## 相关内容

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-wireless`
