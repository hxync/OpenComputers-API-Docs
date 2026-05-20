# screen

## 概述

`screen` 组件是屏幕暴露出的 text buffer 接口。它主要负责屏幕电源状态、键盘关联、精确指针模式以及多方块屏幕几何信息这几个层面的控制与查询。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`screen`
- 常见宿主：屏幕方块，以及以屏幕为宿主的文本缓冲组件

## 作用

- 查询屏幕当前是否点亮。
- 打开或关闭屏幕。
- 查询当前多方块屏幕的宽高比例。
- 列出连接到该屏幕的键盘。
- 在支持的屏幕上读取或设置高精度鼠标模式。

## 多方块说明

- 多方块屏幕只有在等级、朝向和染色颜色都兼容时才会合并。
- 这里返回的宽高比例是按“方块数量”计算，不是像素分辨率。
- 真正的字符分辨率由绑定的 GPU 决定，不由这个组件直接设置。

## API

### isOn

- 语法：`screen.isOn()`
- 返回值：`boolean`
- 用途：返回屏幕当前是否处于开启显示状态。

### turnOn

- 语法：`screen.turnOn()`
- 返回值：`changed, isOn`
- 用途：打开屏幕。

返回值含义：

- `changed:boolean`：这次调用是否真的改变了电源状态。
- `isOn:boolean`：调用结束后屏幕的最终开关状态。

### turnOff

- 语法：`screen.turnOff()`
- 返回值：`changed, isOn`
- 用途：关闭屏幕。

返回值含义与 `turnOn()` 相同。

### getAspectRatio

- 语法：`screen.getAspectRatio()`
- 返回值：`widthBlocks, heightBlocks`
- 用途：返回当前已合并屏幕的方块宽度和方块高度。

### getKeyboards

- 语法：`screen.getKeyboards()`
- 返回值：`table`
- 用途：返回所有连接到该屏幕的键盘地址列表。

行为：

- 该调用会有一个短暂停顿。
- 对多方块屏幕来说，它会把所有已合并屏幕块相邻连接的键盘一起统计进结果。

### isPrecise

- 语法：`screen.isPrecise()`
- 返回值：`boolean`
- 用途：返回当前鼠标事件是否使用高精度坐标模式。

在高精度模式下，指针事件会使用带小数的坐标，而不是整字符格坐标。

### setPrecise

- 语法：`screen.setPrecise(enabled)`
- 参数：
  - `enabled:boolean`：目标高精度模式状态。
- 返回值：
  - 成功：修改前的高精度模式布尔值
  - 失败：`nil, "unsupported operation"`
- 用途：启用或禁用高精度指针坐标上报。

只有支持 tier 3 深度能力的屏幕才支持这个操作。

## 使用说明

- 指针事件会以 `touch`、`drag`、`drop`、`scroll` 这些信号形式发送给计算机。
- 当高精度模式关闭时，坐标会按从 `1` 开始的字符格返回。
- 当高精度模式开启时，坐标会按 viewport 空间中的浮点值返回。

## 示例

```lua
local component = require("component")
local screen = component.screen

local changed, isOn = screen.turnOn()
print("Changed:", changed, "On:", isOn)

local w, h = screen.getAspectRatio()
print("Block size:", w, h)

print("Attached keyboards:", table.concat(screen.getKeyboards(), ", "))

local old, reason = screen.setPrecise(true)
print("Previous precise mode:", old, reason)
print("Precise mode now:", screen.isPrecise())
```

## 相关内容

- `opencomputers-screen`
- `gpu`
- `keyboard`
