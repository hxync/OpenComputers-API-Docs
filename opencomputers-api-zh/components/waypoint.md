# waypoint

## 概述

`waypoint` 组件暴露的是路标方块上那一段可编辑标签。路标本身主要是给导航升级使用的，导航升级可以发现它们，并把它们作为位置标记提供给机器人、无人机等移动设备。

这个组件接口本身非常小，目的就是让程序可以直接读取或重命名路标，而不必手动打开方块界面。

## 可用性

- 仓库：`opencomputers`
- 组件名：`waypoint`
- 常见宿主：路标方块

## 重要说明

- 导航升级实际检测到的位置是路标**正前方的方块**，而不是路标自身所在的方块。
- 源码里路标标签长度上限是 32 个字符。
- 修改标签时，电脑会短暂停顿。

## 用法

```lua
local component = require("component")
local waypoint = component.waypoint

if not waypoint then
  error("当前环境没有 waypoint 组件")
end
```

## 接口

### getLabel

- 语法：`waypoint.getLabel()`
- 返回值：`string`
- 用途：读取当前路标标签。

标签通常会被导航程序当作行为标记使用，例如表示取货点、出货点、充电点或中转点。

示例：

```lua
print("路标标签:", waypoint.getLabel())
```

### setLabel

- 语法：`waypoint.setLabel(value)`
- 返回值：没有直接返回值
- 用途：修改路标标签。

参数说明：

- `value:string`：新的标签文本。

行为说明：

- 标签超过 32 个字符时会被截断；
- 源码会让电脑暂停 `0.5` 秒。

示例：

```lua
waypoint.setLabel("north_dock_input")
print("新标签:", waypoint.getLabel())
```

## 相关内容

- `navigation`
- `robot`
- `drone`
- `component`
