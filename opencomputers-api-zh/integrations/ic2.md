# ic2

## 概述

本页说明 OpenComputers 为 IndustrialCraft 2 提供的集成接口。内容涵盖作物方块、EU 能量网络设备、质量制造机、传送机，以及多个核反应堆访问入口。

## 可用性

- 依赖: `ic2`
- 标签: `integration-required`

## 组件名

- `mass_fab`
- `crop`
- `energy_conductor`
- `energy_sink`
- `energy_source`
- `energy_storage`
- `ic2_teleporter`
- `reactor`
- `reactor_chamber`
- `reactor_redstone_port`

## 主要用途

- 在 Lua 中读取 IC2 作物属性和环境数值。
- 查询 EU 储能设备和导体的能力边界。
- 通过主反应堆、反应堆舱室或红石端口监控并控制反应堆。
- 用脚本修改传送机目标坐标。
- 跟踪质量制造机充能进度。

## 组件

### mass_fab

#### getProgress()

- 语法: `getProgress(): number`
- 用途: 返回 OC 驱动为质量制造机计算出的进度值。
- 返回值:
  - `100 * storedEnergy / demandedEnergy` 的计算结果。
- 使用说明:
  - 这不是 IC2 单独提供的现成百分比字段，而是驱动根据能量值现场算出来的。
  - 驱动不会再做额外归一化或钳制。

### crop

`crop` 组件对应 IC2 作物方块。

#### getSize()

- 语法: `getSize(): number`
- 用途: 返回当前作物的生长阶段大小。

#### getGrowth()

- 语法: `getGrowth(): number`
- 用途: 返回作物的 growth 属性。

#### getGain()

- 语法: `getGain(): number`
- 用途: 返回作物的 gain 属性。

#### getResistance()

- 语法: `getResistance(): number`
- 用途: 返回作物的 resistance 属性。

#### getNutrientStorage()

- 语法: `getNutrientStorage(): number`
- 用途: 返回内部储存的营养值。

#### getHydrationStorage()

- 语法: `getHydrationStorage(): number`
- 用途: 返回内部储存的水分值。

#### getWeedExStorage()

- 语法: `getWeedExStorage(): number`
- 用途: 返回内部储存的 Weed-EX 数值。

#### getHumidity()

- 语法: `getHumidity(): number`
- 用途: 返回作物周围当前的湿度值。

#### getNutrients()

- 语法: `getNutrients(): number`
- 用途: 返回作物周围当前的营养值。

#### getAirQuality()

- 语法: `getAirQuality(): number`
- 用途: 返回作物周围当前的空气质量值。

#### getName()

- 语法: `getName(): string`
- 用途: 返回当前种植的 crop card 名称。
- 使用说明:
  - 驱动直接读取 `tileEntity.getCrop().name()`。

#### getRootsLength()

- 语法: `getRootsLength(): number`
- 用途: 返回当前 crop card 报告的根系长度。

#### getTier()

- 语法: `getTier(): number`
- 用途: 返回当前 crop card 的等级。

#### maxSize()

- 语法: `maxSize(): number`
- 用途: 返回当前 crop card 的最大尺寸。

#### canGrow()

- 语法: `canGrow(): boolean`
- 用途: 返回当前 crop card 在这个地块上是否还能继续生长。

#### getOptimalHavestSize()

- 语法: `getOptimalHavestSize(): number`
- 用途: 返回当前 crop card 的最佳收获尺寸。
- 使用说明:
  - 暴露给 Lua 的方法名就是 `Havest` 这个拼写，因为驱动源码本身就是这样写的。

#### 使用说明

- 作物相关接口全部是只读接口。
- 其中一部分值来自作物方块本体，而 `getName()`、`getRootsLength()`、`getTier()`、`maxSize()`、`canGrow()`、`getOptimalHavestSize()` 来自当前种植的 crop card。

### energy_conductor

#### getConductionLoss()

- 语法: `getConductionLoss(): number`
- 用途: 返回导体的线路损耗值。

#### getConductorBreakdownEnergy()

- 语法: `getConductorBreakdownEnergy(): number`
- 用途: 返回导体击穿阈值。

#### getInsulationBreakdownEnergy()

- 语法: `getInsulationBreakdownEnergy(): number`
- 用途: 返回绝缘层击穿阈值。

#### getInsulationEnergyAbsorption()

- 语法: `getInsulationEnergyAbsorption(): number`
- 用途: 返回绝缘层最多可吸收的能量值。

### energy_sink

#### getSinkTier()

- 语法: `getSinkTier(): number`
- 用途: 返回这个设备接受 EU 输入时使用的 IC2 sink 等级。

### energy_source

#### getOfferedEnergy()

- 语法: `getOfferedEnergy(): number`
- 用途: 返回该能量源当前可提供的能量值。

### energy_storage

#### getCapacity()

- 语法: `getCapacity(): number`
- 用途: 返回最大储能容量。

#### getOutput()

- 语法: `getOutput(): number`
- 用途: 返回当前输出上限。

#### getStored()

- 语法: `getStored(): number`
- 用途: 返回当前储能量。

### ic2_teleporter

#### setCoords(x, y, z)

- 语法: `setCoords(x: number, y: number, z: number)`
- 用途: 设置传送机目标坐标。
- 参数:
  - `x`、`y`、`z`: 目标坐标。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 只有三个参数全部是整数时，目标坐标才会真正写入。
  - 如果其中任意一个参数不是整数，回调会直接结束，不会报错，也不会改动目标。

### reactor

`reactor`、`reactor_chamber` 和 `reactor_redstone_port` 绝大多数接口相同，差别主要在于它们如何找到真实反应堆，以及在没有有效目标时各自怎么处理。

#### 共享槽位信息表

当槽位中有物品时，`getSlotInfo(x, y)` 返回一个表，可能包含：

- `item`: 原始物品堆。
- `canStoreHeat`: 仅反应堆组件才有。
- `heat`: 仅支持热量信息的反应堆组件才有。
- `maxHeat`: 仅支持热量信息的反应堆组件才有。

如果槽位为空，则返回 `nil`。

#### reactor.getHeat()

- 语法: `getHeat(): number`
- 用途: 返回当前反应堆热量。

#### reactor.getMaxHeat()

- 语法: `getMaxHeat(): number`
- 用途: 返回爆堆前的最大热量阈值。

#### reactor.getReactorEnergyOutput()

- 语法: `getReactorEnergyOutput(): number`
- 用途: 返回原始反应堆能量输出值，不乘基础 EU/t。

#### reactor.getReactorEUOutput()

- 语法: `getReactorEUOutput(): number`
- 用途: 返回基础 EU/t 输出值。

#### reactor.producesEnergy()

- 语法: `producesEnergy(): boolean`
- 用途: 返回反应堆当前是否处于产能状态。

#### reactor.setActive(active)

- 语法: `setActive(active: boolean): boolean`
- 用途: 通过红石状态启用或停用反应堆。
- 返回值:
  - 设置后的红石状态。
  - 如果拿不到有效的电力反应堆实例，则返回 `false`。

#### reactor.getSlotInfo(x, y)

- 语法: `getSlotInfo(x: number, y: number): table | nil`
- 用途: 返回指定反应堆网格槽位的物品信息。

### reactor_chamber

`reactor_chamber` 通过连接的反应堆舱室转发访问。

#### reactor_chamber.getHeat()

- 语法: `getHeat(): number`
- 用途: 返回所连接反应堆的当前热量。
- 返回值:
  - 正常时返回真实热量。
  - 如果舱室没有连到有效反应堆，则返回 `0`。

#### reactor_chamber.getMaxHeat()

- 语法: `getMaxHeat(): number`
- 用途: 返回所连接反应堆的最大热量阈值。
- 返回值:
  - 正常时返回真实值。
  - 如果舱室没有连到有效反应堆，则返回 `0`。

#### reactor_chamber.getReactorEnergyOutput()

- 语法: `getReactorEnergyOutput(): number`
- 用途: 返回所连接反应堆的原始能量输出。
- 返回值:
  - 正常时返回真实值。
  - 如果舱室没有连到有效反应堆，则返回 `0`。

#### reactor_chamber.getReactorEUOutput()

- 语法: `getReactorEUOutput(): number`
- 用途: 返回所连接反应堆的基础 EU/t 输出。
- 使用说明:
  - 这一项和上面几个不同，驱动在读取前没有先做空目标回退保护。

#### reactor_chamber.producesEnergy()

- 语法: `producesEnergy(): boolean`
- 用途: 返回所连接反应堆当前是否正在产能。
- 返回值:
  - 正常时返回真实状态。
  - 如果舱室没有连到有效反应堆，则返回 `false`。

#### reactor_chamber.setActive(active)

- 语法: `setActive(active: boolean): boolean`
- 用途: 通过舱室改写反应堆红石状态。
- 返回值:
  - 设置后的红石状态。
  - 如果拿不到有效的电力反应堆实例，则返回 `false`。

#### reactor_chamber.getSlotInfo(x, y)

- 语法: `getSlotInfo(x: number, y: number): table | nil`
- 用途: 通过舱室读取指定槽位的物品信息。
- 返回值:
  - 有物品时返回共享槽位信息表。
  - 如果没有连到反应堆，或者槽位为空，则返回 `nil`。

### reactor_redstone_port

`reactor_redstone_port` 会先解析出它连接的是主反应堆还是反应堆舱室，再基于真实目标提供接口，并额外多一个放热量查询。

#### reactor_redstone_port.getHeat()

- 语法: `getHeat(): number`
- 用途: 返回目标反应堆当前热量。
- 返回值:
  - 正常时返回真实值。
  - 没有有效目标时返回 `0`。

#### reactor_redstone_port.getMaxHeat()

- 语法: `getMaxHeat(): number`
- 用途: 返回目标反应堆最大热量阈值。
- 返回值:
  - 正常时返回真实值。
  - 没有有效目标时返回 `0`。

#### reactor_redstone_port.getReactorEnergyOutput()

- 语法: `getReactorEnergyOutput(): number`
- 用途: 返回目标反应堆原始能量输出。
- 返回值:
  - 正常时返回真实值。
  - 没有有效目标时返回 `0`。

#### reactor_redstone_port.getReactorEUOutput()

- 语法: `getReactorEUOutput(): number`
- 用途: 返回目标反应堆的基础 EU/t 输出。
- 使用说明:
  - 和 chamber 版本一样，这一项不是按“无目标直接回 0”的方式写的。

#### reactor_redstone_port.producesEnergy()

- 语法: `producesEnergy(): boolean`
- 用途: 返回目标反应堆当前是否正在产能。
- 返回值:
  - 正常时返回真实状态。
  - 没有有效目标时返回 `false`。

#### reactor_redstone_port.setActive(active)

- 语法: `setActive(active: boolean): boolean`
- 用途: 通过红石端口改写目标反应堆的红石状态。
- 返回值:
  - 设置后的红石状态。
  - 如果拿不到有效的电力反应堆实例，则返回 `false`。

#### reactor_redstone_port.getEmitHeat()

- 语法: `getEmitHeat(): number`
- 用途: 返回反应堆对外放出的热量，尤其适合流体反应堆监控。
- 返回值:
  - 如果目标是电力反应堆实例，则返回其 `EmitHeat`。
  - 否则返回 `0`。

#### reactor_redstone_port.getSlotInfo(x, y)

- 语法: `getSlotInfo(x: number, y: number): table | nil`
- 用途: 通过红石端口读取指定槽位的物品信息。
- 返回值:
  - 有物品时返回共享槽位信息表。
  - 如果没有有效目标，或者槽位为空，则返回 `nil`。

## 示例

```lua
local component = require("component")

if component.isAvailable("crop") then
  local crop = component.crop
  print("Crop:", crop.getName(), "size", crop.getSize(), "/", crop.maxSize())
  print("Can grow:", crop.canGrow())
end

if component.isAvailable("reactor") then
  local reactor = component.reactor
  print("Heat:", reactor.getHeat(), "/", reactor.getMaxHeat())
  print("EU/t:", reactor.getReactorEUOutput())
end
```

## 相关内容

- `gregtech`
- `mekanism`
