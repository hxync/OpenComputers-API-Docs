# opencomputers-screen

## 概述

本页说明叠加在基础 `screen` 组件之上的屏幕方块专用接口。这组接口用于控制玩家与屏幕交互时，普通点击和潜行点击到底是打开图形界面，还是直接作为触摸输入发送给程序。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`screen`
- 底层实现：`li.cil.oc.common.component.Screen`
- 常见宿主：已放置的屏幕方块

## 用途

- 查询当前是否启用了触摸模式反转。
- 切换普通点击与潜行点击在屏幕交互中的默认行为。

## 触摸模式说明

默认情况下，若屏幕连接了键盘，玩家普通点击通常会优先打开屏幕 GUI，潜行点击则更偏向于直接发送触摸事件。

启用触摸模式反转后，这两种交互的默认解释会交换：

- 普通点击更偏向于发送触摸输入。
- 潜行点击更偏向于打开 GUI。

最终效果仍然会受到屏幕是否连接键盘、当前界面状态以及 OpenComputers 屏幕交互规则影响，但这个开关就是脚本层面可控制的反转入口。

## API

### isTouchModeInverted

- 语法：`screen.isTouchModeInverted()`
- 返回值：`boolean`
- 用途：返回当前是否已经启用触摸模式反转。

### setTouchModeInverted

- 语法：`screen.setTouchModeInverted(value)`
- 参数：
  - `value:boolean`：希望设置的新状态。`true` 表示启用反转，`false` 表示恢复默认模式。
- 返回值：`boolean`
- 用途：设置触摸模式反转状态，并返回修改前的旧状态。
- 说明：
  - 该调用只修改当前屏幕方块的交互行为。
  - 修改后会立即影响之后的玩家点击操作。

## 使用示例

```lua
local component = require("component")
local screen = component.screen

local previous = screen.setTouchModeInverted(true)
print("Previous inverted state:", previous)
print("Current inverted state:", screen.isTouchModeInverted())
```

### 示例用途

适合在自定义终端、信息面板或触摸控制台中切换“更适合点按操作”与“更适合维护调试”这两种交互模式。

## 相关内容

- `screen`
- `keyboard`
