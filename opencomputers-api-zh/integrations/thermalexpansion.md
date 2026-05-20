# thermalexpansion

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Thermal Expansion 集成接口。当前集成提供对兼容 Thermal Expansion 灯的颜色控制能力。

## 可用性

- 依赖: `thermalexpansion`
- 标签: `integration-required`

## 组件名

- `lamp`

## 主要用途

- 用脚本修改 Thermal Expansion 灯的颜色。
- 构建由自动化逻辑驱动的状态灯、警示灯或装饰性灯光效果。

## 组件

### lamp

`lamp` 用于兼容的 Thermal Expansion 灯方块。

#### `setColor(color)`

- 语法: `setColor(color: number): boolean`
- 用途: 修改灯的颜色。
- 参数:
  - `color: number`
    直接传给 Thermal Expansion 灯方块 `setColor(int)` 的整数颜色值。
- 返回值:
  - 底层 `setColor` 方法的返回值。
  - 如果反射调用失败、目标方法不存在或目标方块拒绝这次调用，则这里会得到 `nil`。
- 说明:
  - OpenComputers 不会对这个值做范围校验、颜色空间转换或 RGB 拆分，只是把 Lua 传入的整数原样交给目标方块。
  - 这不是 OC GPU 使用的 `0xRRGGBB` 颜色接口，而是 Thermal Expansion 灯自身使用的颜色编码。

## 示例

```lua
local component = require("component")

if component.isAvailable("lamp") then
  local lamp = component.lamp
  lamp.setColor(3)
end
```

## 相关内容

- `cofh`
- `redstone`
