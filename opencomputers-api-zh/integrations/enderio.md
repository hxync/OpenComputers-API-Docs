# enderio

## 概述

本页说明 OpenComputers 与 Computronics 为 Ender IO 提供的集成接口。内容涵盖机器状态、电容库、经验存储方块、侧面 IO 配置、电力监视器、通用储能、红石控制、telepad、维度收发器、真空箱和天气方尖碑。

## 可用性

- 依赖: `enderio`
- 标签: `integration-required`

## 组件名

- `enderio_machine`
- `capacitor_bank`
- `experience_tile`
- `io_config_tile`
- `power_monitor`
- `power_storage`
- `redstone_tile`
- `telepad`
- `dimensional_transceiver`
- `vacuum_chest`
- `weather_obelisk`

## 主要用途

- 监控 Ender IO 机器是否活跃、当前进度如何。
- 调整电容库吞吐量和红石门控模式。
- 读取或修改每个侧面的 IO 配置。
- 在 Lua 中配置 telepad 目标位置。
- 自动化管理维度收发器频道成员关系。
- 监控电力网络、经验容器、真空范围和天气装置。

## 组件

### enderio_machine

`enderio_machine` 是一个共享组件族。某个具体机器会暴露哪部分接口，取决于它实现了哪些 Ender IO 接口。

#### isActive()

- 语法: `isActive(): boolean`
- 用途: 返回机器当前是否处于活动状态。
- 使用说明:
  - 这个接口来自 `AbstractMachineEntity` 驱动。

#### getPowerPerTick()

- 语法: `getPowerPerTick(): number`
- 用途: 返回机器每 tick 的耗能值，读取自 `getPowerUsePerTick()`。
- 使用说明:
  - 这个接口来自 `AbstractPoweredMachineEntity` 驱动。

#### getProgress()

- 语法: `getProgress(): number`
- 用途: 返回机器当前的内部进度值。
- 使用说明:
  - 这个接口来自 `IProgressTile` 驱动。
  - 返回值就是 Ender IO 内部保存的原始进度，不保证是规范化百分比。

### capacitor_bank

`capacitor_bank` 组件对应 Ender IO 电容库。

#### getAverageInputPerTick()

- 语法: `getAverageInputPerTick(): number`
- 用途: 返回最近的平均每 tick 输入能量。
- 返回值:
  - 如果该电容库属于一个 bank 网络，则返回网络平均值。
  - 如果当前没有网络对象，则返回 `0`。

#### getAverageOutputPerTick()

- 语法: `getAverageOutputPerTick(): number`
- 用途: 返回最近的平均每 tick 输出能量。
- 返回值:
  - 如果该电容库属于一个 bank 网络，则返回网络平均值。
  - 如果当前没有网络对象，则返回 `0`。

#### getAverageChangePerTick()

- 语法: `getAverageChangePerTick(): number`
- 用途: 返回最近的平均每 tick 净变化量。
- 返回值:
  - 如果该电容库属于一个 bank 网络，则返回网络平均值。
  - 如果当前没有网络对象，则返回 `0`。

#### setMaxInput(max)

- 语法: `setMaxInput(max: number)`
- 用途: 修改最大输入速率。
- 参数:
  - `max`: 新的最大输入值。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 如果电容库已经加入网络，写入会作用在网络对象上，而不是单个方块实例上。

#### setMaxOutput(max)

- 语法: `setMaxOutput(max: number)`
- 用途: 修改最大输出速率。
- 参数:
  - `max`: 新的最大输出值。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 如果电容库已经加入网络，写入会作用在网络对象上，而不是单个方块实例上。

#### getInputMode()

- 语法: `getInputMode(): string`
- 用途: 返回输入侧当前的红石控制模式。

#### getOutputMode()

- 语法: `getOutputMode(): string`
- 用途: 返回输出侧当前的红石控制模式。

#### setInputMode(mode)

- 语法: `setInputMode(mode: string)`
- 用途: 修改输入侧的红石控制模式。
- 参数:
  - `mode`: `ignore`、`on`、`off`、`never` 之一。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 非法值会报错 `No valid Redstone mode given`。

#### setOutputMode(mode)

- 语法: `setOutputMode(mode: string)`
- 用途: 修改输出侧的红石控制模式。
- 参数:
  - `mode`: `ignore`、`on`、`off`、`never` 之一。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 非法值会报错 `No valid Redstone mode given`。

#### redstone_modes

- 语法: `redstone_modes`
- 用途: 返回支持的全部红石控制模式列表。
- 返回值:
  - 一个带索引的 Lua 表，内容是 `ignore`、`on`、`off`、`never`。

### experience_tile

`experience_tile` 组件对应实现了 `IHaveExperience` 的 Ender IO 方块。

#### getExperience()

- 语法: `getExperience(): number`
- 用途: 返回经验容器当前保存的总经验值。

#### getExperienceLevels()

- 语法: `getExperienceLevels(): number`
- 用途: 返回当前折算出来的经验等级数。

#### getMaxExperience()

- 语法: `getMaxExperience(): number`
- 用途: 返回该方块最多可存储的经验值。

### io_config_tile

`io_config_tile` 组件对应支持每侧 IO 配置的 Ender IO 机器。

#### getIOMode(side)

- 语法: `getIOMode(side: number): string`
- 用途: 返回某个侧面当前的 IO 模式。
- 参数:
  - `side`: 从 `1` 到 `6` 的侧面编号。
- 返回值:
  - 一个小写模式名。
- 使用说明:
  - 驱动会把 Lua 里的侧面编号转换成从 `0` 开始的 `ForgeDirection`。
  - 超出 `1..6` 的值会报错 `side needs to be between 1 and 6`。

#### setIOMode(side, mode)

- 语法: `setIOMode(side: number, mode: string)`
- 用途: 修改某个侧面的 IO 模式。
- 参数:
  - `side`: 从 `1` 到 `6` 的侧面编号。
  - `mode`: `none`、`pull`、`push`、`push_pull`、`disabled` 之一。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 非法侧面会报错 `side needs to be between 1 and 6`。
  - 非法模式会报错 `No valid IO mode given`。

#### io_modes

- 语法: `io_modes`
- 用途: 返回支持的全部 IO 模式列表。
- 返回值:
  - 一个带索引的 Lua 表，内容是 `none`、`pull`、`push`、`push_pull`、`disabled`。

### power_monitor

`power_monitor` 组件对应 Ender IO 电力监视器。

#### getPowerInConduits()

- 语法: `getPowerInConduits(): number`
- 用途: 返回导管网络当前储存的能量。

#### getMaxPowerInConduits()

- 语法: `getMaxPowerInConduits(): number`
- 用途: 返回导管网络最大可储存能量。

#### getPowerInCapBanks()

- 语法: `getPowerInCapBanks(): number`
- 用途: 返回连接电容库当前储存的能量。

#### getMaxPowerInCapBanks()

- 语法: `getMaxPowerInCapBanks(): number`
- 用途: 返回连接电容库最大可储存能量。

#### getPowerInMachines()

- 语法: `getPowerInMachines(): number`
- 用途: 返回连接机器当前储存的能量。

#### getMaxPowerInMachines()

- 语法: `getMaxPowerInMachines(): number`
- 用途: 返回连接机器最大可储存能量。

#### getAverageEnergySent()

- 语法: `getAverageEnergySent(): number`
- 用途: 返回最近的平均能量发送值。

#### getAverageEnergyReceived()

- 语法: `getAverageEnergyReceived(): number`
- 用途: 返回最近的平均能量接收值。

#### isEngineControlEnabled()

- 语法: `isEngineControlEnabled(): boolean`
- 用途: 返回 Engine Control 当前是否启用。

#### setEngineControlEnabled(control)

- 语法: `setEngineControlEnabled(control: boolean)`
- 用途: 启用或禁用 Engine Control。
- 参数:
  - `control`: 目标 Engine Control 状态。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - ComputerCraft 分支里的报错文案误写成了 `first argument needs to be a number`，但真实要求的类型仍然是布尔值。

#### getStartLevel()

- 语法: `getStartLevel(): number`
- 用途: 返回开始输出红石时使用的阈值。

#### setStartLevel(level)

- 语法: `setStartLevel(level: number)`
- 用途: 修改开始输出红石时使用的阈值。
- 参数:
  - `level`: 新阈值。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 驱动会把这个值按浮点数保存。

#### getStopLevel()

- 语法: `getStopLevel(): number`
- 用途: 返回停止输出红石时使用的阈值。

#### setStopLevel(level)

- 语法: `setStopLevel(level: number)`
- 用途: 修改停止输出红石时使用的阈值。
- 参数:
  - `level`: 新阈值。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 驱动会把这个值按浮点数保存。

### power_storage

`power_storage` 组件对应实现了 `IPowerStorage` 的通用 Ender IO 储能方块。

#### getMaxInput()

- 语法: `getMaxInput(): number`
- 用途: 返回最大输入速率。

#### getMaxOutput()

- 语法: `getMaxOutput(): number`
- 用途: 返回最大输出速率。

#### getEnergyStored()

- 语法: `getEnergyStored(): number`
- 用途: 返回当前储能量。

#### getMaxEnergyStored()

- 语法: `getMaxEnergyStored(): number`
- 用途: 返回最大储能容量。

#### 使用说明

- 这些接口都是只读接口。
- OC 驱动在一条内部路径里复用了类似 capacitor bank 的环境命名，但对外语义上这组接口描述的是通用储能能力。

### redstone_tile

`redstone_tile` 组件对应实现了通用红石模式控制的 Ender IO 方块。

#### getRedstoneMode()

- 语法: `getRedstoneMode(): string`
- 用途: 返回当前红石控制模式。

#### setRedstoneMode(mode)

- 语法: `setRedstoneMode(mode: string)`
- 用途: 修改红石控制模式。
- 参数:
  - `mode`: `ignore`、`on`、`off`、`never` 之一。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - 非法值会报错 `No valid Redstone mode given`。

#### redstone_modes

- 语法: `redstone_modes`
- 用途: 返回支持的全部红石模式列表。
- 返回值:
  - 一个带索引的 Lua 表，内容是 `ignore`、`on`、`off`、`never`。

### telepad

`telepad` 组件对应 Ender IO telepad。

#### getX()

- 语法: `getX(): number`
- 用途: 返回当前配置的目标 X 坐标。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用，否则会报错 `telepad is not a valid structure`。

#### getY()

- 语法: `getY(): number`
- 用途: 返回当前配置的目标 Y 坐标。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### getZ()

- 语法: `getZ(): number`
- 用途: 返回当前配置的目标 Z 坐标。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### getCoords()

- 语法: `getCoords(): number, number, number`
- 用途: 返回当前配置的目标坐标。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### getDimension()

- 语法: `getDimension(): number`
- 用途: 返回当前配置的目标维度 ID。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### setX(x)

- 语法: `setX(x: number): number | nil, string?`
- 用途: 修改目标 X 坐标。
- 参数:
  - `x`: 新的目标 X 坐标。
- 返回值:
  - 成功时返回新的 X 坐标。
  - 如果配置禁止修改坐标，则返回 `nil, "not enabled in config"`。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### setY(y)

- 语法: `setY(y: number): number | nil, string?`
- 用途: 修改目标 Y 坐标。
- 参数:
  - `y`: 新的目标 Y 坐标。
- 返回值:
  - 成功时返回新的 Y 坐标。
  - 如果配置禁止修改坐标，则返回 `nil, "not enabled in config"`。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### setZ(z)

- 语法: `setZ(z: number): number | nil, string?`
- 用途: 修改目标 Z 坐标。
- 参数:
  - `z`: 新的目标 Z 坐标。
- 返回值:
  - 成功时返回新的 Z 坐标。
  - 如果配置禁止修改坐标，则返回 `nil, "not enabled in config"`。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### setCoords(x, y, z)

- 语法: `setCoords(x: number, y: number, z: number): number, number, number | nil, string?`
- 用途: 一次修改全部目标坐标。
- 参数:
  - `x`、`y`、`z`: 新的目标坐标。
- 返回值:
  - 成功时返回新的 `x, y, z`。
  - 如果配置禁止修改坐标，则返回 `nil, "not enabled in config"`。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### setDimension(dimension)

- 语法: `setDimension(dimension: number): number | nil, string?`
- 用途: 修改目标维度 ID。
- 参数:
  - `dimension`: 新的维度 ID。
- 返回值:
  - 成功时返回新的维度 ID。
  - 如果配置禁止修改维度，则返回 `nil, "not enabled in config"`。
- 使用说明:
  - 只有 telepad 已经处于有效网络结构中时才能调用。

#### teleport()

- 语法: `teleport()`
- 用途: 激活 telepad 并传送符合条件的实体。
- 返回值:
  - 成功时没有有意义的返回值。
- 使用说明:
  - OC 驱动会先检查 telepad 是否为有效结构，否则报错 `telepad is not a valid structure`。

#### isValid()

- 语法: `isValid(): boolean`
- 用途: 返回 telepad 当前是否处于有效网络结构中。

### dimensional_transceiver

`dimensional_transceiver` 组件对应 Ender IO 维度收发器。

#### getSendChannels(channelType)

- 语法: `getSendChannels(channelType: string): table`
- 用途: 返回该收发器当前针对指定类型配置的发送频道列表。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
- 返回值:
  - 一个带索引的 Lua 表，内容是频道名。
- 使用说明:
  - 非法频道类型会报错 `No valid channel type given`。

#### setSendChannel(channelType, name, shouldSend)

- 语法: `setSendChannel(channelType: string, name: string, shouldSend: boolean): boolean`
- 用途: 把一个频道加入或移出本地发送配置。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
  - `name`: 频道名。
  - `shouldSend`: 传 `true` 表示加入，传 `false` 表示移除。
- 返回值:
  - 如果本次修改真正应用成功，则返回 `true`。
  - 如果在对应来源列表中找不到这个频道名，则返回 `false`。

#### getReceiveChannels(channelType)

- 语法: `getReceiveChannels(channelType: string): table`
- 用途: 返回该收发器当前针对指定类型配置的接收频道列表。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
- 返回值:
  - 一个带索引的 Lua 表，内容是频道名。
- 使用说明:
  - 非法频道类型会报错 `No valid channel type given`。

#### setReceiveChannel(channelType, name, shouldReceive)

- 语法: `setReceiveChannel(channelType: string, name: string, shouldReceive: boolean): boolean`
- 用途: 把一个频道加入或移出本地接收配置。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
  - `name`: 频道名。
  - `shouldReceive`: 传 `true` 表示加入，传 `false` 表示移除。
- 返回值:
  - 如果本次修改真正应用成功，则返回 `true`。
  - 如果在对应来源列表中找不到这个频道名，则返回 `false`。

#### addChannel(channelType, name)

- 语法: `addChannel(channelType: string, name: string): boolean`
- 用途: 在全局公共频道注册表中新增一个频道。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
  - `name`: 频道名。
- 返回值:
  - 新建成功时返回 `true`。
  - 如果同类型同名频道已经存在，或者类型无法解析，则返回 `false`。

#### removeChannel(channelType, name)

- 语法: `removeChannel(channelType: string, name: string): boolean`
- 用途: 从全局公共频道注册表中删除一个频道。
- 参数:
  - `channelType`: `power`、`item`、`fluid`、`rail` 之一。
  - `name`: 频道名。
- 返回值:
  - 删除成功时返回 `true`。
  - 找不到匹配频道时返回 `false`。

#### isChannelExisting(channelType, name)

- 语法: `isChannelExisting(channelType: string, name: string): boolean`
- 用途: 返回公共频道是否存在。

#### getChannels(channelType)

- 语法: `getChannels(channelType: string): table`
- 用途: 返回指定类型下当前注册的全部公共频道。
- 返回值:
  - 一个带索引的 Lua 表，内容是频道名。

#### isSendChannel(channelType, name)

- 语法: `isSendChannel(channelType: string, name: string): boolean`
- 用途: 返回当前收发器是否已配置为向指定频道发送。

#### isReceiveChannel(channelType, name)

- 语法: `isReceiveChannel(channelType: string, name: string): boolean`
- 用途: 返回当前收发器是否已配置为从指定频道接收。

#### channel_types

- 语法: `channel_types`
- 用途: 返回全部可用频道类型列表。
- 返回值:
  - 一个带索引的 Lua 表，内容是 `power`、`item`、`fluid`、`rail`。

### vacuum_chest

`vacuum_chest` 组件对应 Ender IO 真空箱。

#### getRange()

- 语法: `getRange(): number`
- 用途: 返回当前吸取范围。

#### setRange(range)

- 语法: `setRange(range: number)`
- 用途: 修改吸取范围。
- 参数:
  - `range`: 新的范围值。
- 返回值:
  - 成功时没有有意义的返回值。

### weather_obelisk

`weather_obelisk` 组件对应 Ender IO 天气方尖碑。

#### canActivate(task)

- 语法: `canActivate(task: number): boolean[, string]`
- 用途: 返回指定天气任务当前是否可以启动。
- 参数:
  - `task`: 天气模式的数字 ID。
- 返回值:
  - 对于合法模式 ID，返回 `true` 或 `false`。
  - 对于越界 ID，返回 `false, "invalid weather mode. needs to be between 1 and N"`。
- 使用说明:
  - 支持的模式可通过 `weather_modes` 查询。

#### activate()

- 语法: `activate(): boolean`
- 用途: 立即尝试启动一个天气任务。
- 返回值:
  - 成功启动任务时返回 `true`。
  - 如果已有任务在运行，或者当前前置条件不满足，则返回 `false`。
- 使用说明:
  - 要成功启动，方尖碑必须具备所需流体、烟花、没有活动任务，并且已经结束冷却。

#### weather_modes

- 语法: `weather_modes`
- 用途: 返回天气模式名到数字 ID 的映射表。
- 返回值:
  - 一个 Lua 表，包含：
    - `clear = 1`
    - `rain = 2`
    - `storm = 3`

## 示例

```lua
local component = require("component")

if component.isAvailable("capacitor_bank") then
  local bank = component.capacitor_bank
  print("Cap bank input mode:", bank.getInputMode())
  print("Average change:", bank.getAverageChangePerTick())
end

if component.isAvailable("telepad") then
  local telepad = component.telepad
  if telepad.isValid() then
    local x, y, z = telepad.getCoords()
    print("Telepad target:", x, y, z, "dim", telepad.getDimension())
  end
end
```

## 相关内容

- `opencomputers-redstone-vanilla`
- `computronics`
