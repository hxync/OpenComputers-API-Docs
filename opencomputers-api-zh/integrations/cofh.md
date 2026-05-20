# cofh

## 概述

本页说明 OpenComputers 向 Lua 暴露的 CoFH 集成接口。可用组件覆盖 Redstone Flux 储能、CoFH 机器能量信息、末影传输频道、红石控制状态以及安全权限检查。

## 可用性

- 依赖: `cofh`
- 标签: `integration-required`

## 组件名

- `ender_energy`
- `ender_fluid`
- `ender_item`
- `energy_handler`
- `energy_info`
- `redstone_control`
- `secure_tile`

## 主要用途

- 读取兼容 CoFH 的机器中的 RF 储能。
- 按不同方向读取机器可见的储能与容量。
- 读取 CoFH 机器 GUI 风格的能量统计。
- 读取和修改末影传输频道频率。
- 用 Lua 改变 CoFH 机器的红石控制模式。
- 检查安全方块的所有者和访问权限。

## 组件

### ender_energy

`ender_energy` 用于实现 `IEnderEnergyHandler` 的 CoFH 末影能量传输方块。

#### `canReceiveEnergy()`

- 语法: `canReceiveEnergy(): boolean`
- 用途: 返回该方块是否能从末影频道接收能量。
- 返回值:
  - `true` 表示允许接收。
  - `false` 表示不允许。

#### `canSendEnergy()`

- 语法: `canSendEnergy(): boolean`
- 用途: 返回该方块是否能向末影频道发送能量。
- 返回值:
  - `true` 表示允许发送。
  - `false` 表示不允许。

#### `getFrequency()`

- 语法: `getFrequency(): number`
- 用途: 返回当前末影频道频率。
- 返回值:
  - 当前频率值。

#### `setFrequency(frequency)`

- 语法: `setFrequency(frequency: number): boolean`
- 用途: 修改末影频道频率。
- 参数:
  - `frequency: number`
    新的频道频率。
- 返回值:
  - `true` 表示修改成功。
  - `false` 表示目标方块拒绝该频率。

#### `getChannelString()`

- 语法: `getChannelString(): string`
- 用途: 返回人类可读的频道名称。
- 返回值:
  - 频道显示字符串。

### ender_fluid

`ender_fluid` 用于实现 `IEnderFluidHandler` 的 CoFH 末影流体传输方块。

#### `canReceiveFluid()`

- 语法: `canReceiveFluid(): boolean`
- 用途: 返回该方块是否能通过末影频道接收流体。
- 返回值:
  - `true` 表示允许输入流体。
  - `false` 表示不允许。

#### `canSendFluid()`

- 语法: `canSendFluid(): boolean`
- 用途: 返回该方块是否能通过末影频道发送流体。
- 返回值:
  - `true` 表示允许输出流体。
  - `false` 表示不允许。

#### `getFrequency()`

- 语法: `getFrequency(): number`
- 用途: 返回当前末影流体频道频率。
- 返回值:
  - 当前频率值。

#### `setFrequency(frequency)`

- 语法: `setFrequency(frequency: number): boolean`
- 用途: 修改末影流体频道频率。
- 参数:
  - `frequency: number`
    新的频道频率。
- 返回值:
  - `true` 表示修改成功。
  - `false` 表示失败。

#### `getChannelString()`

- 语法: `getChannelString(): string`
- 用途: 返回该方块显示的频道名称。
- 返回值:
  - 频道名称字符串。

### ender_item

`ender_item` 用于实现 `IEnderItemHandler` 的 CoFH 末影物品传输方块。

#### `canReceiveItems()`

- 语法: `canReceiveItems(): boolean`
- 用途: 返回该方块是否能通过末影频道接收物品。
- 返回值:
  - `true` 表示允许输入物品。
  - `false` 表示不允许。

#### `canSendItems()`

- 语法: `canSendItems(): boolean`
- 用途: 返回该方块是否能通过末影频道发送物品。
- 返回值:
  - `true` 表示允许输出物品。
  - `false` 表示不允许。

#### `getFrequency()`

- 语法: `getFrequency(): number`
- 用途: 返回当前末影物品频道频率。
- 返回值:
  - 当前频率值。

#### `setFrequency(frequency)`

- 语法: `setFrequency(frequency: number): boolean`
- 用途: 修改末影物品频道频率。
- 参数:
  - `frequency: number`
    新的频道频率。
- 返回值:
  - `true` 表示修改成功。
  - `false` 表示失败。

#### `getChannelString()`

- 语法: `getChannelString(): string`
- 用途: 返回当前配置的频道名称。
- 返回值:
  - 频道名称字符串。

### energy_handler

`energy_handler` 是 CoFH Redstone Flux 储能的通用组件，会出现在实现 `IEnergyHandler`、`IEnergyProvider` 或 `IEnergyReceiver` 的方块上。

#### `getEnergyStored([direction])`

- 语法: `getEnergyStored([direction: number]): number`
- 用途: 返回指定方向看到的当前储能。
- 参数:
  - `direction: number`
    可选 ForgeDirection 方向编号。省略时驱动会使用 `ForgeDirection.UNKNOWN`，也就是常见的 `6`。
- 返回值:
  - 该方向上的当前 RF 储量。
- 说明:
  - 如果同一台机器不同面有不同容量或输入输出限制，就应当使用这个接口按面读取。

#### `getMaxEnergyStored([direction])`

- 语法: `getMaxEnergyStored([direction: number]): number`
- 用途: 返回指定方向看到的最大储能容量。
- 参数:
  - `direction: number`
    可选方向编号，默认未知方向。
- 返回值:
  - 该方向上的最大 RF 容量。

#### `isEnergyProvider()`

- 语法: `isEnergyProvider(): boolean`
- 用途: 返回当前方块是否可以作为能量提供端。
- 返回值:
  - 对支持提供能量行为的 `IEnergyProvider` 或完整 `IEnergyHandler` 返回 `true`。
  - 对纯接收端返回 `false`。

#### `isEnergyReceiver()`

- 语法: `isEnergyReceiver(): boolean`
- 用途: 返回当前方块是否可以作为能量接收端。
- 返回值:
  - 接收端能力存在时返回 `true`。
  - 仅提供端时返回 `false`。
- 说明:
  - 完整的 `IEnergyHandler` 可能同时返回 `true` 和 `true`。

### energy_info

`energy_info` 用于实现 `IEnergyInfo` 的 CoFH 方块。

#### `getEnergy()`

- 语法: `getEnergy(): number`
- 用途: 返回当前已存储能量。
- 返回值:
  - 当前 RF 储量。

#### `getEnergyPerTick()`

- 语法: `getEnergyPerTick(): number`
- 用途: 返回机器当前每 tick 的能量速率。
- 返回值:
  - 方块报告的每 tick 能量值。
- 说明:
  - 对不同机器，这个值可能表示发电速率，也可能表示耗电速率。

#### `getMaxEnergy()`

- 语法: `getMaxEnergy(): number`
- 用途: 返回最大储能。
- 返回值:
  - 最大 RF 存储量。

#### `getMaxEnergyPerTick()`

- 语法: `getMaxEnergyPerTick(): number`
- 用途: 返回最大每 tick 能量速率。
- 返回值:
  - 方块报告的最大 RF/t。

### redstone_control

`redstone_control` 用于实现 `IRedstoneControl` 的 CoFH 方块。

#### `getControlDisable()`

- 语法: `getControlDisable(): boolean`
- 用途: 返回机器当前是否处于 `DISABLED` 控制模式。
- 返回值:
  - `true` 表示红石控制完全禁用。
  - `false` 表示当前是其他模式。

#### `getControlSetting()`

- 语法: `getControlSetting(): number`
- 用途: 以枚举序号形式返回当前控制模式。
- 返回值:
  - 当前控制模式的数值编号。

#### `getControlSettingName()`

- 语法: `getControlSettingName(): string`
- 用途: 返回当前控制模式名称。
- 返回值:
  - 枚举名称字符串，例如 `DISABLED` 或其他 CoFH 控制模式名。

#### `getControlName(index)`

- 语法: `getControlName(index: number): string`
- 用途: 把控制模式编号转换成对应名称。
- 参数:
  - `index: number`
    控制模式枚举序号。
- 返回值:
  - 对应模式名称字符串。

#### `isPowered()`

- 语法: `isPowered(): boolean`
- 用途: 返回方块当前是否接收到红石信号。
- 返回值:
  - `true` 表示已通电。
  - `false` 表示未通电。

#### `setControlDisable()`

- 语法: `setControlDisable(): boolean`
- 用途: 强制把机器设置成 `DISABLED` 控制模式。
- 返回值:
  - 设置完成后始终返回 `true`。

#### `setControlSetting(state)`

- 语法: `setControlSetting(state: number|string): boolean`
- 用途: 修改 CoFH 红石控制模式。
- 参数:
  - `state: number|string`
    可以传控制模式的枚举序号，也可以传精确的枚举名称。
- 返回值:
  - `true` 表示设置成功。
- 说明:
  - 当传字符串时，必须完全匹配底层 `ControlMode` 枚举名。

### secure_tile

`secure_tile` 用于实现 `ISecurable` 的 CoFH 安全方块。

#### `canPlayerAccess(name)`

- 语法: `canPlayerAccess(name: string): boolean`
- 用途: 返回指定玩家是否有权限访问该方块。
- 参数:
  - `name: string`
    要检查的玩家名称。
- 返回值:
  - `true` 表示允许访问。
  - `false` 表示不允许访问。
- 说明:
  - 驱动会从当前服务器玩家列表中解析这个玩家档案后再做权限检查。

#### `getAccess()`

- 语法: `getAccess(): string`
- 用途: 返回当前安全访问模式。
- 返回值:
  - 首字母大写的 CoFH 访问模式名。
- 说明:
  - 实际可见值取决于所安装的 CoFH 版本，常见有 `Public`、`Restricted`、`Private` 等。

#### `getOwnerName()`

- 语法: `getOwnerName(): string`
- 用途: 返回方块所有者名称。
- 返回值:
  - 所有者玩家名。

## 示例

```lua
local component = require("component")

if component.isAvailable("energy_handler") then
  local energy = component.energy_handler
  print("Stored RF:", energy.getEnergyStored())
  print("Capacity:", energy.getMaxEnergyStored())
end

if component.isAvailable("redstone_control") then
  local control = component.redstone_control
  print("Mode:", control.getControlSettingName())
  print("Powered:", control.isPowered())
end
```

## 相关内容

- `thermalexpansion`
- `buildcraft`
