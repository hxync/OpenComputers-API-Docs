# draconicevolution

## 概述

本页说明 OpenComputers 对 Draconic Evolution 扩展 RF 储能方块的集成接口。

## 可用性

- 依赖: `draconicevolution`
- 标签: `integration-required`

## 组件名

- `draconic_storage`

## 主要用途

- 读取当前储能量和最大储能量。
- 为大容量电力系统制作监控面板。
- 在储能接近满载或见底时触发自动化逻辑。

## 组件

### draconic_storage

`draconic_storage` 组件对应实现了 `IExtendedRFStorage` 的方块实体。

#### `getEnergyStored()`

- 语法: `getEnergyStored(): number`
- 用途: 返回当前储存的能量值。
- 返回值:
  - 当前已存储能量。

#### `getMaxEnergyStored()`

- 语法: `getMaxEnergyStored(): number`
- 用途: 返回最大能量容量。
- 返回值:
  - 最大可存储能量。
- 说明:
  - 这两个接口都是只读接口。
  - 返回值直接来自目标方块的储能实现。
  - 数值单位就是该方块内部使用的 RF 风格能量单位。

#### 示例

```lua
local component = require("component")
local storage = component.draconic_storage

local stored = storage.getEnergyStored()
local capacity = storage.getMaxEnergyStored()

print("Energy:", stored, "/", capacity)
print(string.format("Fill: %.2f%%", stored / capacity * 100))
```

## 相关内容

- `mekanism`
- `cofh`
