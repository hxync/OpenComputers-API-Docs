# fsp

## 概述

本页说明 OpenComputers 与 Computronics 为 Flaxbeard's Steam Power 提供的集成接口。它增加了一个只读组件，用来查询蒸汽传输方块的当前状态。

## 可用性

- 依赖: `fsp`
- 标签: `integration-required`

## 组件名

- `steam_transporter`

## 主要用途

- 监控蒸汽管线是否为空、是否满载、是否有压力。
- 在锅炉停机或供汽不足时触发告警逻辑。
- 把蒸汽实时数据接入监控面板或控制循环。

## 组件

### steam_transporter

`steam_transporter` 组件对应实现了 FSP `ISteamTransporter` 接口的方块。

#### getSteamPressure()

- 语法: `getSteamPressure(): number`
- 用途: 返回当前蒸汽压力。
- 返回值:
  - 底层 `getPressure()` 返回的压力值。
- 使用说明:
  - 这是只读查询接口。
  - OC 驱动不会对这个值做额外换算，而是直接返回底层运输器提供的结果。

#### getSteamCapacity()

- 语法: `getSteamCapacity(): number`
- 用途: 返回该运输器可容纳的最大蒸汽量。
- 返回值:
  - 底层 `getCapacity()` 返回的容量值。
- 使用说明:
  - 这是只读查询接口。

#### getSteamAmount()

- 语法: `getSteamAmount(): number`
- 用途: 返回当前已经储存在运输器中的蒸汽量。
- 返回值:
  - 底层 `getSteam()` 返回的蒸汽存量。
- 使用说明:
  - 这是只读查询接口。
  - 和 `getSteamPressure()` 一起看，最适合判断空管、堵塞或供汽不足。

## 示例

```lua
local component = require("component")
local transporter = component.steam_transporter

local pressure = transporter.getSteamPressure()
local amount = transporter.getSteamAmount()
local capacity = transporter.getSteamCapacity()

print(string.format("Steam: %d / %d", amount, capacity))
print("Pressure:", pressure)

if amount == 0 then
  print("Warning: the line is empty.")
end
```

## 相关内容

- `railcraft`
- `buildcraft`
