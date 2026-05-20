# mekanism

## 概述

本页说明 OpenComputers 对 Mekanism 严格能量存储接口的集成文档。

## 可用性

- 依赖: `mekanism`
- 标签: `integration-required`

## 组件名

- `mekanism_machine`

## 主要用途

- 读取 Mekanism 设备当前储能。
- 监控电池缓存和耗能机器的电量状态。
- 在机器能量不足时触发自动化逻辑。

## 组件

### mekanism_machine

`mekanism_machine` 组件对应实现了 `IStrictEnergyStorage` 的方块实体。

#### `getEnergyStored()`

- 语法: `getEnergyStored(): number`
- 用途: 返回当前储存的能量值。
- 返回值:
  - 当前已存储能量。

#### `getMaxEnergyStored()`

- 语法: `getMaxEnergyStored(): number`
- 用途: 返回最大储能容量。
- 返回值:
  - 最大储能容量。
- 说明:
  - 这些数值来自 Mekanism 自己的 strict energy API，而不是 RF 兼容包装层。
  - 两个接口都是只读接口，适合定期轮询。

#### 示例

```lua
local component = require("component")
local machine = component.mekanism_machine

local stored = machine.getEnergyStored()
local capacity = machine.getMaxEnergyStored()

print("Energy:", stored, "/", capacity)
```

## 相关内容

- `draconicevolution`
- `cofh`
