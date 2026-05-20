# particle

## 概述

`particle` 组件是 Computronics 的粒子发射卡。它可以在相对于宿主的位置生成粒子效果，并且可以为这些粒子指定随机速度或显式速度。

## 可用性

- 仓库：`computronics`
- 组件名：`particle`
- 常见宿主：安装了 Computronics 粒子卡的兼容 OC 设备

## 用法

```lua
local component = require("component")
local particle = component.particle
```

## 范围与能量

- 源码中默认允许的最大距离是 `Config.FX_RANGE`，默认值为 `256`。
- 如果 `FX_RANGE` 为负数，则源码会关闭这个距离检查。
- 能量消耗为 `Config.FX_ENERGY_COST * distance`。
- 源码默认能量系数是 `0.2`。

## 接口

### spawn

- 语法：
  - `particle.spawn(name, x, y, z[, defaultVelocity])`
  - `particle.spawn(name, x, y, z[, vx, vy, vz])`
- 返回：
  - 成功：`true`
  - 失败：`false` 或 `false, reason`
- 用途：在相对于宿主的位置生成一个粒子效果。

参数说明：

- `name:string`：粒子名称。
- `x:number`、`y:number`、`z:number`：相对于宿主位置的偏移量。
- `defaultVelocity:number` 可选：用于生成随机速度的高斯散布系数。
- `vx:number`、`vy:number`、`vz:number` 可选：显式指定三个方向的速度分量。

行为说明：

- 粒子名称长度不能超过 `Short.MAX_VALUE`。
- 坐标偏移会被限制到 `-65536` 到 `65536`。
- 距离根据相对偏移向量的欧几里得长度计算。
- 如果距离超过配置上限，则返回 `false, "out of range"`。
- 如果名称过长，则返回 `false, "name too long"`。
- 如果连接器无法支付所需能量，则返回 `false`。
- 只提供 `defaultVelocity` 时，三个方向速度都会按 `defaultVelocity * 高斯随机数` 生成。
- 如果显式提供了三个速度分量，则会直接使用这些值。

示例：

```lua
assert(particle.spawn("smoke", 0, 1, 0, 0.02))
assert(particle.spawn("reddust", 0, 1, 0, 0, 0.05, 0))
```

## 相关内容

- `component`
- `computronics-computronics-explode`
