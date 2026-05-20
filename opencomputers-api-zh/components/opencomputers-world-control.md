# opencomputers-world-control

## 概述

本页记录代理类设备例如机器人使用的简单相邻方块检测回调。

## 可用性

- 仓库：`opencomputers`
- 常见 Lua 组件名：通常作为 `robot` 或其他代理类设备的一部分出现
- 底层实现特征：`WorldControl`

## API

### detect

- 语法：`device.detect(side)`
- 返回值：`boolean, string`
- 用途：检查指定侧面的相邻方块空间里到底是什么。

返回值含义：

- 第一个值表示这个方向在交互意义上是否被视为“有东西占据”或“被阻挡”。
- 第二个值是一个描述检测结果的分类字符串。

这版源码中已知的分类字符串有：

- `air`
- `entity`
- `liquid`
- `replaceable`
- `passable`
- `solid`

解释说明：

- `air` 会返回 `false, "air"`。
- `entity`、`passable` 和 `solid` 会返回 `true` 以及对应标签。
- `liquid` 和 `replaceable` 是否返回真，取决于一次类似破坏权限检查事件的结果。

## 示例

```lua
local robot = require("robot")
local sides = require("sides")

local blocked, kind = robot.detect(sides.front)
print("Blocked:", blocked)
print("Kind:", kind)
```

## 相关内容

- `opencomputers-inventory-world-control`
- `robot`
