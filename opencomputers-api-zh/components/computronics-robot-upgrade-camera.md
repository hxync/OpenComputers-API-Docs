# camera

## 概述

机器人上的 `camera` 组件是 Computronics 提供的短距离测距传感器。它会沿一条射线探测，并返回该方向上第一个实心方块的距离。

这个组件很适合做避障、对齐、简单贴墙移动，以及检查头顶或脚下的空间。

## 可用性

- 仓库：`computronics`
- 组件名：`camera`
- 常见宿主：安装了 Computronics 摄像头升级的机器人

## 用法

```lua
local component = require("component")
local camera = component.camera

if not camera then
  error("未安装 camera 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 射线模型

- 默认最大探测距离为 `32` 格。
- 可选的 `x`、`y` 偏移会改变射线方向。
- 每个偏移值都必须位于 `-1.0` 到 `1.0` 之间。
- 如果射线在最大距离内没有击中方块，接口会返回 `-1`。

机器人版本支持三种方向：

- 按机器人当前朝向向前探测
- 竖直向上探测
- 竖直向下探测

## 接口

### distance

- 语法：`camera.distance([x, y])`
- 返回值：`number`
- 用途：返回机器人前方第一块命中方块的距离。

参数说明：

- `x:number` 可选：水平方向视角偏移。
- `y:number` 可选：垂直方向视角偏移。

行为说明：

- 不传参数时，会沿正前方直射。
- 如果任一偏移值超出 `-1.0` 到 `1.0`，这次扫描不会命中任何目标，返回 `-1`。
- 如果在最大距离内没有找到方块，也会返回 `-1`。

示例：

```lua
local d = camera.distance()
if d >= 0 then
  print("前方墙体距离:", d)
else
  print("前方最大距离内没有探测到方块")
end
```

示例：略微向右探测。

```lua
print("右偏视角距离:", camera.distance(0.4, 0.0))
```

### distanceUp

- 语法：`camera.distanceUp([x, y])`
- 返回值：`number`
- 用途：返回机器人上方第一块命中方块的距离。

参数说明：

- `x:number` 可选：在向上视平面中的横向偏移。
- `y:number` 可选：在向上视平面中的第二偏移量。

适合用来检测天花板、升降设备对接以及垂直净空。

示例：

```lua
local ceiling = camera.distanceUp()
if ceiling >= 0 then
  print("天花板距离:", ceiling)
end
```

### distanceDown

- 语法：`camera.distanceDown([x, y])`
- 返回值：`number`
- 用途：返回机器人下方第一块命中方块的距离。

参数说明：

- `x:number` 可选：在向下视平面中的横向偏移。
- `y:number` 可选：在向下视平面中的第二偏移量。

适合检测悬崖、寻找地面以及判断下落深度。

示例：

```lua
local floor = camera.distanceDown()
if floor < 0 then
  print("探测范围内没有地面")
else
  print("地面距离:", floor)
end
```

## 说明

- 这些接口检测的是方块，不是实体。
- 射线起点会略微偏出机器人本体，以避免宿主方块本身干扰测距结果。

## 相关内容

- `component`
- `radar`
