# camera

## 概述

方块版 `camera` 组件是 Computronics 提供的固定式测距仪。它会沿摄像头方块当前朝向发射一条射线，并返回命中的第一个实心方块距离。

与机器人版本相比，这个方块版只能向前探测，但非常适合装在门口、走廊、对接位和自动化产线上做固定传感器。

## 可用性

- 仓库：`computronics`
- 组件名：`camera`
- 常见宿主：放置在世界中的 Computronics 摄像头方块

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
- 可选 `x`、`y` 偏移会调整视线方向。
- 每个偏移值都必须位于 `-1.0` 到 `1.0` 之间。
- 如果最大距离内没有命中方块，接口返回 `-1`。

该方块始终沿放置后的朝向进行探测。

## 接口

### distance

- 语法：`camera.distance([x, y])`
- 返回值：`number`
- 用途：返回摄像头命中的第一个方块距离。

参数说明：

- `x:number` 可选：水平视角偏移。
- `y:number` 可选：垂直视角偏移。

行为说明：

- 不传参数时，摄像头沿正前方探测。
- 如果任一偏移值超出 `-1.0` 到 `1.0`，接口返回 `-1`。
- 如果最大距离内没有命中任何方块，也会返回 `-1`。

示例：

```lua
local d = camera.distance()
if d >= 0 then
  print("检测到方块距离:", d)
else
  print("探测范围内没有方块")
end
```

示例：略微向上取样。

```lua
print("上偏视角距离:", camera.distance(0.0, -0.35))
```

## 说明

- 该组件检测的是方块，不是实体。
- 如果当前环境还启用了摄像头联动红石刷新，那属于方块侧逻辑，不会改变 `distance()` 的返回格式。

## 相关内容

- `component`
- `radar`
