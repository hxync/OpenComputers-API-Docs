# flamingo

## 概述

本页说明 OpenComputers 对 Flamingo 模组提供的小型装饰性交互接口。

## 可用性

- 依赖: `flamingo`
- 标签: `integration-required`

## 组件名

- `flamingo`

## 主要用途

- 通过 Lua 程序触发展示动画。
- 制作趣味装饰装置和互动展品。
- 让场景道具跟随定时器或外部控制信号动作。

## 组件

### flamingo

`flamingo` 组件对应 Flamingo 方块实体。

#### `wiggle()`

- 语法: `wiggle()`
- 用途: 触发 flamingo 的扭动动画。
- 返回值:
  - 这个回调没有有意义的返回值。
- 说明:
  - 这是通过方块事件触发的可见动画，更适合做视觉反馈，而不是数据处理。

#### `getWiggleStrength()`

- 语法: `getWiggleStrength(): number`
- 用途: 返回当前方块保存的扭动强度值。
- 返回值:
  - 当前保存的扭动强度数值。
- 说明:
  - 如果你想避免过于频繁地重复触发动画，可以先读取这个值再决定是否执行 `wiggle()`。

#### 示例

```lua
local component = require("component")
local flamingo = component.flamingo

print("Current wiggle strength:", flamingo.getWiggleStrength())
flamingo.wiggle()
```

## 相关内容

- `armourersworkshop`
