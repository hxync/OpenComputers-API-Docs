# motion_sensor

## 概述

`motion_sensor` 组件用于探测传感器附近活体实体的移动，并在移动超过设定阈值时向电脑注入 `computer.signal` 事件。它非常适合做警报器、安防系统、陷阱逻辑，以及对附近玩家或生物作出响应的自动化。

探测范围不是全图。它只会监视自己周围 8 格半径内的活体实体，会忽略隐身实体，并且要求传感器与目标之间存在视线。

## 可用性

- 仓库：`opencomputers`
- 组件名：`motion_sensor`
- 常见宿主：运动传感器方块，或通过兼容方式接入的设备

## 运行行为

传感器每 10 tick 检查一次运动，并在以下情况下发出名为 `motion` 的信号：

- 某个活体实体第一次进入可检测范围；
- 某个已经被跟踪的实体移动距离超过当前灵敏度阈值。

信号负载里会包含相对于传感器中心点的偏移坐标：

```lua
"motion", dx, dy, dz
```

如果配置里启用了用户名附带功能，事件末尾还会额外追加实体名称：

```lua
"motion", dx, dy, dz, name
```

## 用法

```lua
local component = require("component")
local motion = component.motion_sensor

if not motion then
  error("当前环境没有 motion_sensor 组件")
end
```

事件循环示例：

```lua
local event = require("event")

while true do
  local _, _, dx, dy, dz, name = event.pull("motion")
  print("检测到移动:", dx, dy, dz, name or "")
end
```

## 接口

### getSensitivity

- 语法：`motion.getSensitivity()`
- 返回值：`number`
- 用途：读取当前移动判定阈值。

数值越小，传感器越敏感；数值越大，则同一个已跟踪实体必须发生更明显的位移才会再次触发信号。

示例：

```lua
print("当前灵敏度:", motion.getSensitivity())
```

### setSensitivity

- 语法：`motion.setSensitivity(value)`
- 返回值：`number`
- 用途：修改移动判定阈值，并返回旧值。

参数说明：

- `value:number`：希望设置的灵敏度阈值。

行为说明：

- 源码会把该值限制到最小 `0.2`；
- 组件本身没有额外写死上限。

示例：

```lua
local old = motion.setSensitivity(0.25)
print("旧灵敏度:", old)
print("新灵敏度:", motion.getSensitivity())
```

## 相关内容

- `component`
- `event`
- `redstone`
