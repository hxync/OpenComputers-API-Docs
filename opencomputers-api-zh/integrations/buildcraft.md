# buildcraft

## 概述

本页说明 OpenComputers 与 Computronics 为 BuildCraft 提供的 Lua 集成接口。可用组件覆盖管道状态读取、热量型机器监控、无人机停靠站以及可切换控制模式的方块。

## 可用性

- 依赖: `buildcraft`
- 标签: `integration-required`

## 组件名

- `bc_pipe`
- `heatable_tile`
- `docking`
- `bc_controllable`

## 主要用途

- 读取管道连接状态、门控状态和导线状态。
- 监控 BuildCraft 热量型机器的工作热区间。
- 让无人机停靠到管道站并向物品管道投放物品。
- 读取和修改机器的控制模式。

## 组件

### bc_pipe

`bc_pipe` 组件对应 BuildCraft 管道方块实体。

#### `getPipeType()`

- 语法: `getPipeType(): string | nil, string?`
- 用途: 返回当前管道的类型名。
- 返回值:
  - 成功时返回管道类型枚举名。
  - 如果底层调用失败，则返回 `nil, "none"`。
- 说明:
  - 这个值适合用来区分物品管道、流体管道、能量管道等类别。

#### `isPipeConnected(side)`

- 语法: `isPipeConnected(side: number): boolean`
- 用途: 检查指定方向是否连接到其他对象。
- 参数:
  - `side: number`
    ForgeDirection 方向编号。
- 返回值:
  - `true` 表示该方向已连接。
  - `false` 表示未连接，或底层读取失败。
- 说明:
  - 无效方向不会抛给 Lua 一个可见错误，而是直接得到 `false`。

#### `isWired(color)`

- 语法: `isWired(color: string): boolean`
- 用途: 检查当前管道是否安装了指定颜色的导线。
- 参数:
  - `color: string`
    BuildCraft `PipeWire` 枚举名，通常应传入全大写颜色名。
- 返回值:
  - `true` 表示已安装该颜色导线。
  - `false` 表示未安装，或颜色名无效。

#### `isWireActive(color)`

- 语法: `isWireActive(color: string): boolean`
- 用途: 检查指定颜色导线当前是否处于激活状态。
- 参数:
  - `color: string`
    BuildCraft `PipeWire` 枚举名，通常应传入全大写颜色名。
- 返回值:
  - `true` 表示该导线当前已激活。
  - `false` 表示未激活，或颜色名无效。
- 说明:
  - `isWired()` 只检查导线是否存在，`isWireActive()` 则检查它当前是否真的导通。

#### `hasGate(side)`

- 语法: `hasGate(side: number): boolean`
- 用途: 检查指定方向是否安装了 gate。
- 参数:
  - `side: number`
    ForgeDirection 方向编号。
- 返回值:
  - `true` 表示该面存在 gate。
  - `false` 表示不存在，或底层读取失败。

### heatable_tile

`heatable_tile` 组件对应实现了 `IHeatable` 的 BuildCraft 方块。

#### `getMinHeatValue()`

- 语法: `getMinHeatValue(): number`
- 用途: 返回最小有效热量值。
- 返回值:
  - 该机器开始进入可工作热区间的下限值。

#### `getIdealHeatValue()`

- 语法: `getIdealHeatValue(): number`
- 用途: 返回理想工作热量值。
- 返回值:
  - 机器最适合维持的热量目标值。

#### `getMaxHeatValue()`

- 语法: `getMaxHeatValue(): number`
- 用途: 返回最大安全热量值。
- 返回值:
  - 超过后通常应视为过热风险上限。

#### `getCurrentHeatValue()`

- 语法: `getCurrentHeatValue(): number`
- 用途: 返回机器当前热量值。
- 返回值:
  - 当前实时热量。
- 说明:
  - 这个接口主要用于引擎或其他依赖热量区间运转的 BuildCraft 机器。
  - 实际监控时，通常更应该把它和 `getIdealHeatValue()` 一起比较，而不是只看最大值。

### docking

`docking` 组件来自 Computronics 的 BuildCraft 无人机停靠升级，用于让 drone 与管道停靠站交互。

#### `dock()`

- 语法: `dock(): boolean[, string]`
- 用途: 让无人机开始尝试停靠到可用停靠站，优先检查正下方。
- 返回值:
  - 成功时返回 `true`。
  - 失败时返回 `false, reason`。
- 说明:
  - 如果无人机还在移动，会返回 `false, "drone is still moving"`。
  - 如果附近没有空闲停靠站，会返回 `false, "no non-occupied station found"`。
  - 调用成功后只是开始停靠流程，不代表当 tick 已完全停稳。

#### `dropItem(slot[, maxAmount[, color]])`

- 语法: `dropItem(slot: number[, maxAmount: number[, color: number]]): number[, string]`
- 用途: 在已停靠状态下，把无人机背包中的物品投入连接的物品管道。
- 参数:
  - `slot: number`
    无人机背包槽位，从 `1` 开始计数。
  - `maxAmount: number`
    最大投入数量，底层会自动限制在 `0` 到 `64` 之间，默认 `64`。
  - `color: number`
    可选 BuildCraft 物品颜色编号，只有二级及以上无人机才会真正生效。
- 返回值:
  - 成功时返回实际投入的物品数量。
  - 失败时返回 `0, reason`。
- 说明:
  - 只有在无人机已经成功停靠到物品管道后才可用。
  - 常见失败原因包括 `drone is still docking`、`drone is not docked`、`cannot inject items into pipe`、`invalid pipe type`、`invalid/empty slot`。
  - 如果指定颜色但无人机不是二级机体，颜色参数会被忽略。

#### `release()`

- 语法: `release(): boolean[, string]`
- 用途: 让无人机从当前停靠站释放。
- 返回值:
  - 成功时返回 `true`。
  - 失败时返回 `0, reason`。
- 说明:
  - 如果无人机还未完成停靠，会返回 `0, "drone is still docking"`。
  - 如果当前根本没有停靠成功，会返回 `0, "drone is not docked"`。

### bc_controllable

`bc_controllable` 组件对应实现了 `IControllable` 的 BuildCraft 方块。

#### `getControlMode()`

- 语法: `getControlMode(): string`
- 用途: 返回当前控制模式名称。
- 返回值:
  - BuildCraft `IControllable.Mode` 枚举名。

#### `acceptsControlMode(mode)`

- 语法: `acceptsControlMode(mode: string): boolean`
- 用途: 检查该方块是否接受指定控制模式。
- 参数:
  - `mode: string`
    BuildCraft `IControllable.Mode` 的精确枚举名。
- 返回值:
  - `true` 表示可接受。
  - `false` 表示该机器不支持这个模式。
- 说明:
  - 如果字符串不是合法枚举名，底层会直接抛出参数错误。

#### `setControlMode(mode)`

- 语法: `setControlMode(mode: string)`
- 用途: 把方块切换到指定控制模式。
- 参数:
  - `mode: string`
    BuildCraft `IControllable.Mode` 的精确枚举名。
- 返回值:
  - 这个回调没有额外返回值。
- 说明:
  - `mode` 必须是 `IControllable.Mode.valueOf(...)` 能识别的精确枚举名。
  - 最稳妥的做法是先用 `getControlMode()` 读取当前值，再用 `acceptsControlMode()` 验证候选模式，最后再调用 `setControlMode()`。

## 示例

```lua
local component = require("component")

if component.isAvailable("bc_pipe") then
  local pipe = component.bc_pipe
  print("Pipe type:", pipe.getPipeType())
  print("North connected:", pipe.isPipeConnected(2))
end

if component.isAvailable("heatable_tile") then
  local heatable = component.heatable_tile
  print("Heat:", heatable.getCurrentHeatValue(), "/", heatable.getMaxHeatValue())
end
```

## 相关内容

- `railcraft`
- `appeng`
