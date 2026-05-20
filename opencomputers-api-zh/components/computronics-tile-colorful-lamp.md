# colorful_lamp

## 概述

`colorful_lamp` 用于控制 Computronics 的彩色灯方块。Lua 可以读取或修改这盏灯保存的颜色值，适合做状态指示、机器状态展示以及与 bundled 颜色线配合的显示逻辑。

这盏灯保存的是一种 15 位颜色样式值，而不是像机架灯板那样使用 24 位 `0xRRGGBB` 颜色。

## 可用性

- 仓库：`computronics`
- 组件名：`colorful_lamp`
- 常见宿主：放置在世界中的 Computronics 彩色灯方块

## 用法

```lua
local component = require("component")
local lamp = component.colorful_lamp

if not lamp then
  error("未安装 colorful_lamp 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 颜色模型

- `0` 表示关灯。
- 非 `0` 值表示亮灯。
- 内部实际保存时只保留低 `15` 位。

这盏灯也被设计成可以响应 bundled 布线输入。使用 bundled 输入时，低 15 位中的每一位都对应一条 bundled 颜色线。

## 接口

### getLampColor

- 语法：`lamp.getLampColor()`
- 返回值：`number`
- 用途：返回灯当前保存的颜色值。

示例：

```lua
print("当前灯颜色值:", lamp.getLampColor())
```

### setLampColor

- 语法：`lamp.setLampColor(color)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：修改灯的颜色值。

参数说明：

- `color:number`：灯颜色值。

行为说明：

- `0` 会让灯熄灭。
- 实现层接受非负整数，但最终只会保存低 15 位。
- 虽然参数检查代码允许到 `65535`，但失败提示文本写的是目标范围应为 `0` 到 `32767`。
- 为了让脚本行为稳定可预测，建议把取值控制在 `0` 到 `32767`。

常见失败返回：

- `false, "number must be between 0 and 32767"`

示例：

```lua
assert(lamp.setLampColor(0x1234))
```

示例：关闭灯。

```lua
assert(lamp.setLampColor(0))
```

## 说明

- 如果环境启用了彩色发光功能，方块会直接使用保存的颜色值来决定发光效果。
- 如果当前环境未启用彩色发光，任何非零颜色值依然会让灯以普通亮度点亮。

## 相关内容

- `component`
- `colors`
