# piston

## 概述

`piston` 组件是活塞升级暴露出的 Lua 接口。它允许支持该升级的宿主按原版活塞规则推动一个相邻方块。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`piston`
- 常见宿主：安装了活塞升级的机器人、无人机、平板和微控制器

## 作用

- 让宿主沿指定方向像原版活塞一样尝试伸出并推动方块。
- 在推动成功前先消耗组件能量。
- 推动成功后，会按该升级的实现流程移除被直接作用的目标方块位置。

## 方向规则

- 对机器人、微控制器这类可旋转宿主，不传方向时默认使用宿主正前方。
- 对无人机，不传方向时默认使用 `south`。
- 对平板，推动原点跟随玩家位置；当玩家视角较高并向下推动时，还带有一个向下偏移的特殊处理。

## API

### push

- 语法：`piston.push([side])`
- 参数：
  - `side:number`：可选，目标侧面。
- 返回值：`boolean`
- 用途：尝试推动宿主指定侧面的方块。

返回值含义：

- `true`：推动成功发生。
- `false`：目标本来就是空气、能量不足，或者原版活塞判定这次推动不允许执行。

## 使用说明

- 该升级遵循原版活塞的伸展限制，因此不可推动方块、被阻挡的推动链等情况都会失败。
- 推动成功时，调用上下文会短暂停顿。
- 每次成功推动都会消耗配置中的活塞能量成本。

## 示例

```lua
local component = require("component")
local sides = require("sides")
local piston = component.piston

local ok = piston.push(sides.front)
print("Push succeeded:", ok)
```

## 相关内容

- `robot`
- `microcontroller`
- `tablet`
