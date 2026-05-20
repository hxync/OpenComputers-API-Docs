# drone

## 概述

`drone` 组件提供无人机专用的状态读取和飞行控制接口。它可以读取当前移动状态、调整目标加速度、修改飞行目标位置，以及更新界面状态文字和机翼灯光颜色。

这些回调只会出现在无人机本体上，因此脚本通常运行在无人机自己的机载电脑里，而不是固定电脑上。

## 可用性

- 仓库：`opencomputers`
- 组件名：`drone`
- 常见宿主：无人机

## 用法

```lua
local component = require("component")
local drone = component.drone

if not drone then
  error("当前环境没有 drone 组件")
end
```

## 接口

### getAcceleration

- 语法：`drone.getAcceleration()`
- 返回值：`number`
- 用途：读取无人机当前配置的目标加速度，单位为米每二次方秒。

它返回的是 `setAcceleration` 最终生效后的数值，已经经过内部换算处理。

示例：

```lua
print("当前加速度:", drone.getAcceleration(), "m/s^2")
```

### getLightColor

- 语法：`drone.getLightColor()`
- 返回值：`number`
- 用途：读取当前机翼灯光颜色，返回值为 `0xRRGGBB` 格式的整数 RGB 值。

示例：

```lua
print(string.format("灯光颜色: 0x%06X", drone.getLightColor()))
```

### getMaxVelocity

- 语法：`drone.getMaxVelocity()`
- 返回值：`number`
- 用途：读取无人机的最大飞行速度，单位为米每秒。

这个值表示无人机实体允许达到的速度上限，不是当前实时速度。

示例：

```lua
print("最大速度:", drone.getMaxVelocity(), "m/s")
```

### getOffset

- 语法：`drone.getOffset()`
- 返回值：`number`
- 用途：读取无人机当前位置到当前目标位置之间剩余的距离。

它非常适合写成等待循环，用来判断无人机是否已经飞到目标点。

示例：

```lua
while drone.getOffset() > 0.2 do
  os.sleep(0.1)
end
print("已到达目标位置")
```

### getStatusText

- 语法：`drone.getStatusText()`
- 返回值：`string`
- 用途：读取当前显示在无人机界面上的状态文字。

示例：

```lua
print("状态:", drone.getStatusText())
```

### getVelocity

- 语法：`drone.getVelocity()`
- 返回值：`number`
- 用途：读取无人机当前飞行速度，单位为米每秒。

该值由实体当前的运动向量计算得到。

示例：

```lua
print("当前速度:", drone.getVelocity(), "m/s")
```

### move

- 语法：`drone.move(dx, dy, dz)`
- 返回值：没有直接返回值
- 用途：在当前目标位置基础上，按给定偏移量修改无人机的飞行目标。

参数说明：

- `dx:number`：X 方向相对位移。
- `dy:number`：Y 方向相对位移。
- `dz:number`：Z 方向相对位移。

行为说明：

- 这个接口不会瞬移无人机。
- 它只会修改无人机接下来尝试飞往的目标坐标。
- 实际飞行轨迹仍然取决于无人机物理运动、当前速度和加速度设置。

示例：

```lua
drone.move(0, 3, 0)
while drone.getOffset() > 0.2 do
  os.sleep(0.1)
end
print("已向上移动 3 格")
```

### setAcceleration

- 语法：`drone.setAcceleration(value)`
- 返回值：`number`
- 用途：设置无人机目标加速度，单位为米每二次方秒，并返回最终生效的数值。

参数说明：

- `value:number`：希望设置的加速度。

Lua 接口使用每秒单位，实体内部实际按每刻单位存储，OpenComputers 会自动完成换算。

示例：

```lua
local applied = drone.setAcceleration(4)
print("生效加速度:", applied, "m/s^2")
```

### setLightColor

- 语法：`drone.setLightColor(value)`
- 返回值：`number`
- 用途：修改机翼灯光颜色，并返回最终应用的 RGB 值。

参数说明：

- `value:number`：`0xRRGGBB` 格式的颜色值。

说明：

- 该调用会让当前电脑短暂停顿，源码中的停顿时间为 `0.1` 秒，用来模拟世界交互消耗。

示例：

```lua
drone.setLightColor(0x00FF00)
print("灯光已切换为绿色")
```

### setStatusText

- 语法：`drone.setStatusText(value)`
- 返回值：`string`
- 用途：修改无人机界面显示的状态文字，并返回最终显示的文本。

参数说明：

- `value:string`：要显示的状态内容。

说明：

- 该调用同样会产生一次短暂停顿，源码中的停顿时间为 `0.1` 秒。

示例：

```lua
drone.setStatusText("运输中")
print("新状态:", drone.getStatusText())
```

## 相关内容

- `computer`
- `navigation`
- `modem`
- `robot`
