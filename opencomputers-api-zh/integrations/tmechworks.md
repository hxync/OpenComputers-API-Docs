# tmechworks

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Tinkers' Mechworks 集成接口。

## 可用性

- 依赖: `tmechworks`
- 标签: `integration-required`

## 组件名

- `drawbridge`

## 主要用途

- 读取抽拉桥当前是否已经伸出。
- 在更大的自动化或安全逻辑里使用抽拉桥状态。

## 组件

### drawbridge

`drawbridge` 用于 Tinkers' Mechworks 的抽拉桥。

#### `hasExtended()`

- 语法: `hasExtended(): boolean`
- 用途: 返回抽拉桥当前是否已经伸出。
- 返回值:
  - `true` 表示桥已伸出。
  - `false` 表示未伸出。

## 示例

```lua
local component = require("component")
local bridge = component.drawbridge
print("Extended:", bridge.hasExtended())
```

## 相关内容

- `redstone`
- `vanilla`
