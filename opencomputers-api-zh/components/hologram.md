# hologram

## 概述

`hologram` 组件用于控制全息投影仪的体素缓冲区、调色板、缩放、平移和旋转。它是 OpenComputers 中少数真正意义上的 3D 显示接口之一：你操作的不是文本单元格，而是一个 `48 x 32 x 48` 的体素空间。

一阶全息投影仪只支持体素开关；二阶全息投影仪支持多种颜色值、可编辑调色板，以及程序控制旋转。

## 可用性

- 仓库：`opencomputers`
- 组件名：`hologram`
- 常见宿主：全息投影仪

## 用法

```lua
local component = require("component")
local hologram = component.hologram

if not hologram then
  error("当前环境没有 hologram 组件")
end
```

## 坐标模型

- Lua 坐标全部是 `1` 基坐标。
- 合法范围为：
  - `x = 1..48`
  - `y = 1..32`
  - `z = 1..48`
- 体素值 `0` 表示空。
- 一阶投影仪可用值基本只有 `0` 和 `1`。
- 二阶投影仪可用值是 `0..3`：
  - `0` = 空；
  - `1..3` = 调色板颜色。

非法坐标会触发参数或越界错误；非法体素值会抛出 `"invalid value"`。

## 接口

### clear

- 语法：`hologram.clear()`
- 返回值：没有直接返回值
- 用途：清空整个全息缓冲区。

它会一次性关闭所有体素。

示例：

```lua
hologram.clear()
```

### get

- 语法：`hologram.get(x, y, z)`
- 返回值：`number`
- 用途：读取指定位置当前的体素值。

参数说明：

- `x:number`、`y:number`、`z:number`：体素坐标。

示例：

```lua
print("体素值:", hologram.get(24, 16, 24))
```

### set

- 语法：`hologram.set(x, y, z, value)`
- 返回值：没有直接返回值
- 用途：设置单个体素。

参数说明：

- `x:number`、`y:number`、`z:number`：体素坐标。
- `value:number|boolean`：体素状态或颜色索引。

说明：

- 传入 `true` 等价于值 `1`；
- 传入 `false` 等价于值 `0`；
- 数字值必须处于该投影仪支持的范围内。

示例：

```lua
hologram.set(24, 16, 24, 1)
```

### fill

- 语法：`hologram.fill(x, z[, minY], maxY, value)`
- 返回值：没有直接返回值
- 用途：在某个 `x,z` 列中，填充一段纵向区间。

参数说明：

- `x:number`、`z:number`：列位置。
- `minY:number` 可选：下边界，默认是 `1`。
- `maxY:number`：上边界。
- `value:number|boolean`：要写入的体素值。

行为说明：

- `minY` 和 `maxY` 会被限制在合法的 `1..32` 范围内；
- 如果处理后 `minY > maxY`，会抛出 `"interval is empty"`。

示例：

```lua
hologram.fill(24, 24, 1, 16, 1)
```

省略 `minY` 的示例：

```lua
hologram.fill(24, 24, 32, true)
```

### setRaw

- 语法：`hologram.setRaw(data)`
- 返回值：没有直接返回值
- 用途：用原始字节数组直接替换投影数据。

参数说明：

- `data:string`：字节数组，每个字节代表一个体素颜色值。

重要细节：

- 源码使用的嵌套顺序是 `x, z, y`；
- 这是批量上传大型体素场景最快的方式；
- 上传后源码会让电脑短暂停顿一次。

示例：

```lua
local bytes = {}
for i = 1, 48 * 48 * 32 do
  bytes[i] = string.char(0)
end
hologram.setRaw(table.concat(bytes))
```

### copy

- 语法：`hologram.copy(x, z, sx, sz, tx, tz)`
- 返回值：没有直接返回值
- 用途：按水平偏移量复制一片列区域。

参数说明：

- `x:number`、`z:number`：源区域起始列。
- `sx:number`、`sz:number`：沿 X 和 Z 的区域尺寸。
- `tx:number`、`tz:number`：平移偏移量。

行为说明：

- 这是按“列”复制的，因此每一列的整段高度都会一起被移动；
- 尺寸小于等于 0 时不会执行任何操作；
- 平移量为 0 时也不会做事；
- 较大的复制操作会根据影响面积让电脑暂停更久。

示例：

```lua
hologram.copy(10, 10, 8, 8, 12, 0)
```

### getScale

- 语法：`hologram.getScale()`
- 返回值：`number`
- 用途：读取当前投影缩放值。

### setScale

- 语法：`hologram.setScale(value)`
- 返回值：没有直接返回值
- 用途：修改投影缩放比例。

参数说明：

- `value:number`：期望缩放值。

行为说明：

- 缩放值最小会被限制到 `1/3`；
- 最大值取决于投影仪等级和模组配置；
- 缩放越大，能耗越高。

示例：

```lua
hologram.setScale(1.5)
print("当前缩放:", hologram.getScale())
```

### getTranslation

- 语法：`hologram.getTranslation()`
- 返回值：`number, number, number`
- 用途：读取当前投影偏移量。

### setTranslation

- 语法：`hologram.setTranslation(tx, ty, tz)`
- 返回值：没有直接返回值
- 用途：修改全息图像相对于投影仪的渲染偏移。

参数说明：

- `tx:number`：X 方向水平偏移。
- `ty:number`：垂直偏移。
- `tz:number`：Z 方向水平偏移。

行为说明：

- X 和 Z 会按配置允许的最大偏移量对称限制；
- Y 会被限制到 `0 .. maxTranslation * 2`。

示例：

```lua
hologram.setTranslation(0.2, 0.5, -0.2)
```

### maxDepth

- 语法：`hologram.maxDepth()`
- 返回值：`number`
- 用途：读取该投影仪支持的颜色深度等级。

实际含义：

- 一阶返回 `1`；
- 二阶返回 `2`。

### getPaletteColor

- 语法：`hologram.getPaletteColor(index)`
- 返回值：`number`
- 用途：按普通 `0xRRGGBB` RGB 格式读取一个调色板颜色。

参数说明：

- `index:number`：调色板条目索引，从 `1` 开始。

非法索引会触发越界错误。

### setPaletteColor

- 语法：`hologram.setPaletteColor(index, value)`
- 返回值：`number`
- 用途：修改一个调色板条目，并返回之前保存的值。

参数说明：

- `index:number`：调色板条目索引，从 `1` 开始。
- `value:number`：`0xRRGGBB` 格式 RGB 颜色值。

如果脚本需要确认屏幕上最终可见的颜色，改完后再调用一次 `getPaletteColor(index)` 是最稳妥的做法。

示例：

```lua
hologram.setPaletteColor(1, 0x00FF00)
```

### setRotation

- 语法：`hologram.setRotation(angle, x, y, z)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：设置全息图像的基础旋转角度和旋转轴。

参数说明：

- `angle:number`：旋转角度，单位为度；源码会按 `360` 取模。
- `x:number`、`y:number`、`z:number`：旋转轴向量。

等级说明：

- 一阶投影仪会返回 `nil, "not supported"`；
- 二阶投影仪支持该功能。

### setRotationSpeed

- 语法：`hologram.setRotationSpeed(speed, x, y, z)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：设置持续旋转速度和旋转轴。

参数说明：

- `speed:number`：旋转速度；源码会把它限制在 `-1440 .. 1440`；
- `x:number`、`y:number`、`z:number`：旋转轴向量。

等级说明：

- 一阶投影仪会返回 `nil, "not supported"`；
- 二阶投影仪支持该功能。

### getDimensions

- 语法：`hologram.getDimensions()`
- 返回值：`number, number, number`
- 用途：按 `x, y, z` 顺序返回投影尺寸。

这个接口始终返回 `48, 32, 48`。

示例：

```lua
local w, h, d = hologram.getDimensions()
print("全息尺寸:", w, h, d)
```

## 相关内容

- `component`
- `robot`
- `drone`
