# colors

## 概述

`colors` 是 Computronics 提供的机器人颜色覆盖升级组件。它允许 Lua 给机器人模型附加一层颜色覆盖，适合做队伍标识、机器状态显示或机器人职责区分。

这个升级要么保存一个 24 位颜色值，要么保存一个特殊的重置值，用来让机器人恢复默认外观。

## 可用性

- 仓库：`computronics`
- 组件名：`colors`
- 常见宿主：安装了 Computronics 彩色升级的机器人

## 用法

```lua
local component = require("component")
local colors = component.colors

if not colors then
  error("未安装 colors 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### setColor

- 语法：`colors.setColor(color)`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：给机器人应用新的覆盖颜色。

参数说明：

- `color:number`：`0xRRGGBB` 形式的 24 位 RGB 颜色值。

行为说明：

- 合法取值范围是 `0` 到 `16777215`，也就是 `0xFFFFFF`。
- 修改机器人颜色会消耗组件网络能量。

常见失败返回：

- `false, "not enough energy"`
- `false, "number must be between 0 and 16777215"`

示例：

```lua
assert(colors.setColor(0xFF8800))
```

### getColor

- 语法：`colors.getColor()`
- 返回值：`number`
- 用途：返回机器人当前保存的覆盖颜色。

返回细节：

- 普通颜色会以 24 位整数形式返回。
- `-1` 表示机器人当前使用默认外观，没有自定义覆盖色。

示例：

```lua
local color = colors.getColor()
if color == -1 then
  print("机器人当前使用默认颜色")
else
  print(string.format("机器人颜色: 0x%06X", color))
end
```

### resetColor

- 语法：`colors.resetColor()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：清除自定义覆盖色，让机器人恢复默认外观。

行为说明：

- 底层实际上会把保存颜色改为 `-1`。
- 重置颜色消耗的能量与设置新颜色相同。

常见失败返回：

- `false, "not enough energy"`

示例：

```lua
assert(colors.resetColor())
```

## 示例

```lua
local level = 0.85

if level > 0.75 then
  colors.setColor(0x00CC00)
elseif level > 0.35 then
  colors.setColor(0xFFCC00)
else
  colors.setColor(0xCC0000)
end
```

## 相关内容

- `component`
- `colorful_lamp`
