# gc

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Galacticraft 世界传感器集成接口。

## 可用性

- 依赖: `gc`
- 标签: `integration-required`

## 组件名

- `world_sensor`

## 主要用途

- 读取 Galacticraft 维度中的重力和风力数值。
- 检查当前世界是否拥有可呼吸大气。
- 判断指定大气气体是否存在。

## 组件

### world_sensor

`world_sensor` 由 Galacticraft 世界传感器卡暴露。

#### `getGravity()`

- 语法: `getGravity(): number`
- 用途: 返回当前世界的重力值。
- 返回值:
  - 当前维度的 Galacticraft 重力倍率。
- 说明:
  - 如果宿主世界不是 Galacticraft 世界提供者，则回退为 `1`。

#### `getWindLevel()`

- 语法: `getWindLevel(): number`
- 用途: 返回当前世界的风力等级。
- 返回值:
  - 当前维度的风力值。
- 说明:
  - 如果宿主世界不是 Galacticraft 世界提供者，则回退为 `1`。

#### `hasBreathableAtmosphere()`

- 语法: `hasBreathableAtmosphere(): boolean`
- 用途: 返回当前世界是否拥有可呼吸大气。
- 返回值:
  - `true` 表示无需额外设备即可呼吸。
  - `false` 表示不可呼吸。
- 说明:
  - 在非 Galacticraft 世界中默认返回 `true`。

#### `isGasPresent(gas)`

- 语法: `isGasPresent(gas: string): boolean`
- 用途: 返回当前世界大气中是否存在指定气体。
- 参数:
  - `gas: string`
    气体名称，例如 `oxygen` 或 `nitrogen`。
- 返回值:
  - `true` 表示存在。
  - `false` 表示不存在。
- 说明:
  - 传入的字符串会先转成大写后再匹配 Galacticraft 气体枚举。
  - 在非 Galacticraft 世界中默认返回 `true`。

## 示例

```lua
local component = require("component")
local sensor = component.world_sensor

print("Gravity:", sensor.getGravity())
print("Wind:", sensor.getWindLevel())
print("Breathable:", sensor.hasBreathableAtmosphere())
print("Has oxygen:", sensor.isGasPresent("oxygen"))
```

## 相关内容

- `stargatetech2`
- `vanilla`
