# railcraft

## 概述

本页说明 OpenComputers 与 Computronics 向 Lua 暴露的 Railcraft 集成接口。可用组件覆盖区块锚、充电设备、发射轨道、限速轨道、机车模式轨道、路由轨道与路由表、数字信号盒、车票机以及机车中继等功能。

## 可用性

- 依赖: `railcraft`
- 标签: `integration-required`

## 组件名

- `anchor`
- `boiler_firebox`
- `electric_tile`
- `launcher_track`
- `limiter_track`
- `locomotive_track`
- `powered_track`
- `priming_track`
- `routing_detector`
- `routing_switch`
- `routing_track`
- `digital_controller_box`
- `digital_receiver_box`
- `ticket_machine`
- `locomotive_relay`
- `steam_turbine`

## 主要用途

- 读取区块锚剩余燃料、所有者和类型。
- 读取 Railcraft 电力网络设备的电量、容量、损耗和耗电。
- 自动化控制发射轨、限速轨、机车模式轨、雷管轨、供电轨和路由轨。
- 读取与写入路由检测器、路由扳道机中的路由表。
- 用 Lua 控制 Railcraft 数字信号系统。
- 自动设置金票目的地并批量打印普通车票。
- 读取和修改已绑定电力机车的目的地与状态。

## 组件

### anchor

`anchor` 是 OpenComputers 在 Railcraft 区块锚方块上暴露的组件。

#### `getFuel()`

- 语法: `getFuel(): number`
- 用途: 返回区块锚剩余燃料时间，单位为 tick。
- 返回值:
  - 剩余燃料 tick 数。
- 说明:
  - 这个值就是 Railcraft 内部维持区块加载时实际消耗的剩余时间。
  - 数值很低时说明锚即将停止工作，需要补充燃料。

#### `getOwner()`

- 语法: `getOwner(): string`
- 用途: 返回区块锚所有者名称。
- 返回值:
  - 所有者名称。
  - 如果所有者仍是 Railcraft 的默认内部占位所有者，则不返回值。

#### `getType()`

- 语法: `getType(): string`
- 用途: 返回区块锚类型。
- 返回值:
  - `world`
  - `admin`
  - `personal`
  - `passive`
  - `missing`

#### `getFuelSlotContents()`

- 语法: `getFuelSlotContents(): table`
- 用途: 返回锚燃料槽当前物品。
- 返回值:
  - OpenComputers 物品栈表。
- 说明:
  - 适合在不开 GUI 的情况下检查当前装填的燃料类型。

#### `isDisabled()`

- 语法: `isDisabled(): boolean`
- 用途: 返回区块锚是否被红石信号禁用。
- 返回值:
  - `true` 表示当前被禁用。
  - `false` 表示允许工作。

### boiler_firebox

`boiler_firebox` 是 Computronics 为 Railcraft 锅炉火箱暴露的组件。

#### `isBurning()`

- 语法: `isBurning(): boolean`
- 用途: 返回火箱当前是否正在燃烧燃料。
- 返回值:
  - `true` 表示正在工作。
  - `false` 表示未燃烧。

#### `getTemperature()`

- 语法: `getTemperature(): number`
- 用途: 返回火箱当前温度。
- 返回值:
  - 当前温度值。

#### `getMaxHeat()`

- 语法: `getMaxHeat(): number`
- 用途: 返回火箱支持的最大热量值。
- 返回值:
  - 最大热量值。

### electric_tile

`electric_tile` 用于接入 Railcraft 电力网络的方块。

#### `getCharge()`

- 语法: `getCharge(): number`
- 用途: 返回当前已存储电量。
- 返回值:
  - 当前电量值。

#### `getCapacity()`

- 语法: `getCapacity(): number`
- 用途: 返回最大储电容量。
- 返回值:
  - 最大容量。

#### `getLoss()`

- 语法: `getLoss(): number`
- 用途: 返回每 tick 电量损耗。
- 返回值:
  - 每 tick 损耗值。

#### `getDraw()`

- 语法: `getDraw(): number`
- 用途: 返回每 tick 电量消耗。
- 返回值:
  - 每 tick 耗电值。

### launcher_track

`launcher_track` 用于 Railcraft 发射轨。

#### `getForce()`

- 语法: `getForce(): number`
- 用途: 返回当前发射力度。
- 返回值:
  - 当前力度值。

#### `setForce(force)`

- 语法: `setForce(force: number): boolean[, string]`
- 用途: 设置发射轨用于弹射矿车的力度。
- 参数:
  - `force: number`
    发射力度整数值。
- 返回值:
  - `true` 表示设置成功。
  - `false, 错误信息` 表示数值超出允许范围。
- 说明:
  - 合法范围是 `5` 到 Railcraft 配置中允许的最大力度。
  - 非法值会返回类似 `not a valid force value, needs to be between 5 and ...` 的错误。

### limiter_track

`limiter_track` 用于 Railcraft 限速轨。

#### `setLimit(mode)`

- 语法: `setLimit(mode: number): boolean`
- 用途: 设置限速模式。
- 参数:
  - `mode: number`
    `1` 到 `4` 之间的限速模式编号。
- 返回值:
  - `true` 表示成功。
- 说明:
  - OpenComputers 对非法值会直接抛参数错误。
  - ComputerCraft 会报出类似 `mode needs to be between 1 and 4` 的错误。
  - 返回与传入的都是 Lua 层模式值，不是内部 NBT 原始值。

#### `getLimit()`

- 语法: `getLimit(): number`
- 用途: 返回当前限速模式。
- 返回值:
  - `1` 到 `4` 之间的模式编号。

### locomotive_track

`locomotive_track` 用于 Railcraft 机车控制轨。

#### `setMode(mode)`

- 语法: `setMode(mode: number): boolean`
- 用途: 设置轨道写入机车的运行模式。
- 参数:
  - `mode: number`
    Lua 层模式值，只能是 `0`、`1`、`2`。
- 返回值:
  - `true` 表示成功。
- 说明:
  - `0` 表示 `shutdown`
  - `1` 表示 `idle`
  - `2` 表示 `running`
  - 非法值会触发参数错误。

#### `getMode()`

- 语法: `getMode(): number`
- 用途: 返回当前配置的机车模式。
- 返回值:
  - `0` 表示 `shutdown`
  - `1` 表示 `idle`
  - `2` 表示 `running`

#### `modes`

- 语法: `modes: table`
- 用途: 提供机车模式名称与 Lua 数值的对应表。
- 返回值:
  - 类似 `{ shutdown = 0, idle = 1, running = 2 }` 的表。
- 说明:
  - 在 OpenComputers 文档里它更像一个 getter 属性，而不是普通函数调用。

### powered_track

`powered_track` 用于实现了 Railcraft 供电轨接口的轨道。

#### `isPowered()`

- 语法: `isPowered(): boolean[, string]`
- 用途: 返回轨道当前是否接收到红石信号。
- 返回值:
  - 可访问时返回 `true` 或 `false`。
  - 如果轨道已上锁，则返回 `nil, "track is locked"`。

### priming_track

`priming_track` 用于 Railcraft 雷管轨。

#### `getFuse()`

- 语法: `getFuse(): number`
- 用途: 返回当前引信时间，单位为 tick。
- 返回值:
  - 当前引信时长。

#### `setFuse(fuse)`

- 语法: `setFuse(fuse: number): boolean[, string]`
- 用途: 设置雷管轨使用的引信时间。
- 参数:
  - `fuse: number`
    引信时长，单位 tick。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示超出允许范围。
- 说明:
  - 合法范围是 `0` 到 `500`。

### routing_detector

`routing_detector` 用于 Railcraft 路由检测器。

#### `getRoutingTable()`

- 语法: `getRoutingTable(): table[, string]`
- 用途: 返回检测器中路由表的全部内容。
- 返回值:
  - 成功时返回按数字索引的字符串表。
  - 如果没有路由表、路由表无效，或检测器被锁定，则返回 `false, 错误信息`。
- 说明:
  - 返回表会在分页之间插入 `"{newpage}"` 标记。
  - 最后一页之后不会保留多余的分页标记。

#### `setRoutingTable(routingTable)`

- 语法: `setRoutingTable(routingTable: table): boolean[, string]`
- 用途: 用 Lua 表整体覆盖路由表内容。
- 参数:
  - `routingTable: table`
    以数字索引的字符串表。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示失败。
- 说明:
  - 写入分页时要使用 `"{newline}"` 作为换页标记。
  - 读取时返回的是 `"{newpage}"`，写入时要求 `"{newline}"`，两者不同，使用时要特别注意。
  - 常见失败原因包括 `no routing table found`、`no valid routing table found`、`routing detector is locked`。

#### `getRoutingTableTitle()`

- 语法: `getRoutingTableTitle(): string[, string]`
- 用途: 返回检测器内路由表物品的标题。
- 返回值:
  - 路由表标题。
  - 若缺失或无法访问，则返回 `false, 错误信息`。

#### `setRoutingTableTitle(name)`

- 语法: `setRoutingTableTitle(name: string): boolean[, string]`
- 用途: 修改检测器内路由表的标题。
- 参数:
  - `name: string`
    新标题。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示失败。

### routing_switch

`routing_switch` 用于 Railcraft 路由扳道机。

#### `getRoutingTable()`

- 语法: `getRoutingTable(): table[, string]`
- 用途: 返回扳道机中安装的完整路由表内容。
- 返回值:
  - 按数字索引的字符串表。
  - 若路由表不存在、无效或扳道机已锁定，则返回 `false, 错误信息`。
- 说明:
  - 分页边界以 `"{newpage}"` 返回。

#### `setRoutingTable(routingTable)`

- 语法: `setRoutingTable(routingTable: table): boolean[, string]`
- 用途: 覆盖扳道机中的路由表内容。
- 参数:
  - `routingTable: table`
    按数字索引的字符串表。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示失败。
- 说明:
  - 写入分页时使用 `"{newline}"`。
  - 如果扳道机已锁定，会返回 `false, "routing switch is locked"`。

#### `getRoutingTableTitle()`

- 语法: `getRoutingTableTitle(): string[, string]`
- 用途: 返回扳道机内路由表的标题。
- 返回值:
  - 当前标题。
  - 不可访问时返回 `false, 错误信息`。

#### `setRoutingTableTitle(name)`

- 语法: `setRoutingTableTitle(name: string): boolean[, string]`
- 用途: 重命名扳道机中的路由表。
- 参数:
  - `name: string`
    新标题。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示失败。

### routing_track

`routing_track` 用于 Railcraft 路由轨。

#### `setDestination(destination)`

- 语法: `setDestination(destination: string): boolean[, string]`
- 用途: 把目的地写入轨道中安装的金票。
- 参数:
  - `destination: string`
    要写入的目的地名称。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示轨道没有金票或轨道已锁定。
- 说明:
  - 轨道的第 `0` 号内部槽位必须放有金票。

#### `getDestination()`

- 语法: `getDestination(): string[, string]`
- 用途: 读取轨道内金票保存的目的地。
- 返回值:
  - 目的地字符串。
  - `false, 错误信息` 表示没有金票或轨道已锁定。

### digital_controller_box

`digital_controller_box` 是 Computronics 为 Railcraft 数字信号控制盒暴露的组件。

#### `setAspect(name, aspect)`

- 语法: `setAspect(name: string, aspect: number): boolean[, string]`
- 用途: 为指定名称的所有已配对信号设置显示状态。
- 参数:
  - `name: string`
    已配对信号名称。
  - `aspect: number`
    信号状态编号。
- 返回值:
  - `true` 表示成功。
  - `false, "no valid signal found"` 表示没有该名称的已配对信号。
- 说明:
  - 可用状态值见 `aspects`。

#### `setEveryAspect(aspect)`

- 语法: `setEveryAspect(aspect: number): boolean`
- 用途: 把同一个状态应用到所有已配对信号。
- 参数:
  - `aspect: number`
    来自 `aspects` 表的状态编号。
- 返回值:
  - `true` 表示成功。

#### `unpair(name)`

- 语法: `unpair(name: string): boolean[, string]`
- 用途: 删除指定名称对应的全部配对关系。
- 参数:
  - `name: string`
    已配对信号名称。
- 返回值:
  - `true` 表示成功。
  - `false, "no valid signal found"` 表示名称不存在。

#### `getSignalNames()`

- 语法: `getSignalNames(): table`
- 用途: 返回当前控制盒已知的所有接收端名称。
- 返回值:
  - 包含名称的 Lua 表。

#### `aspects`

- 语法: `aspects: table`
- 用途: 提供 Railcraft 可用信号状态与数值编号的对应关系。
- 返回值:
  - 同时支持名称到编号、编号到名称的双向映射表。
- 说明:
  - 实际导出的状态包括:
    - `green = 1`
    - `blink_yellow = 2`
    - `yellow = 3`
    - `blink_red = 4`
    - `red = 5`

### digital_receiver_box

`digital_receiver_box` 是 Computronics 为 Railcraft 数字信号接收盒暴露的组件。

#### `getAspect(name)`

- 语法: `getAspect(name: string): number[, string]`
- 用途: 返回指定名称信号当前接收到的状态。
- 参数:
  - `name: string`
    已配对信号名称。
- 返回值:
  - 成功时返回状态编号。
  - 若名称不存在，则返回 `nil, "no valid signal found"`。

#### `getMostRestrictiveAspect()`

- 语法: `getMostRestrictiveAspect(): number`
- 用途: 返回当前所有已连接信号中最严格的状态。
- 返回值:
  - 状态编号。

#### `unpair(name)`

- 语法: `unpair(name: string): boolean[, string]`
- 用途: 删除指定控制端名称的全部配对关系。
- 参数:
  - `name: string`
    控制端名称。
- 返回值:
  - `true` 表示成功。
  - `false, "no valid signal found"` 表示名称不存在。

#### `getSignalNames()`

- 语法: `getSignalNames(): table`
- 用途: 返回当前接收盒已知的全部控制端名称。
- 返回值:
  - 名称表。

#### `aspects`

- 语法: `aspects: table`
- 用途: 返回可用信号状态与编号的映射。
- 返回值:
  - 与 `digital_controller_box` 相同的双向状态映射表。

#### 事件: `aspect_changed`

- 语法: `aspect_changed(name, aspectIndex)`
- 用途: 当某个已配对信号状态变化时向程序发送事件。
- 参数:
  - `name: string`
    发生变化的信号名称。
  - `aspectIndex: number`
    新状态编号。

### ticket_machine

`ticket_machine` 是 Computronics 的 Railcraft 车票机组件。

#### `printTicket([amount[, slot]])`

- 语法: `printTicket([amount: number[, slot: number]]): boolean[, string]`
- 用途: 排队打印一张或多张普通车票。
- 参数:
  - `amount: number`
    要打印的票数，默认是 `1`。
  - `slot: number`
    使用的金票槽位，默认使用当前选中槽位。
- 返回值:
  - `true` 表示成功。
  - `false, 错误信息` 表示常见机器故障。
- 说明:
  - 用户可见的票槽编号是 `1` 到 `10`。
  - 机器纸槽里必须有纸。
  - 对应槽位里必须放有已经写好有效目的地的金票。
  - 常见失败信息包括:
    - `machine is already printing`
    - `no paper found in paper slot`
    - `no golden ticket in specified slot`
    - `tickets not enabled in config`
    - `not enough energy`
    - `output slot already contains ticket with different destination`
    - `output slot is too full`

#### `setManualPrintingAllowed(allowed)`

- 语法: `setManualPrintingAllowed(allowed: boolean): boolean`
- 用途: 允许或禁止玩家手动点击打印。
- 参数:
  - `allowed: boolean`
    `true` 表示允许手动打印。
- 返回值:
  - 新的允许状态。

#### `isManualPrintingAllowed()`

- 语法: `isManualPrintingAllowed(): boolean`
- 用途: 返回当前是否允许玩家手动打印。
- 返回值:
  - `true` 表示 GUI 中可以手动打印。
  - `false` 表示手动打印已锁定。

#### `setManualSelectionAllowed(allowed)`

- 语法: `setManualSelectionAllowed(allowed: boolean): boolean`
- 用途: 允许或禁止玩家手动切换当前选中的金票槽位。
- 参数:
  - `allowed: boolean`
    `true` 表示允许手动切换。
- 返回值:
  - 新的允许状态。

#### `isManualSelectionAllowed()`

- 语法: `isManualSelectionAllowed(): boolean`
- 用途: 返回当前是否允许手动选择票槽。
- 返回值:
  - `true` 表示 GUI 中允许切换槽位。
  - `false` 表示手动选择已锁定。

#### `setDestination([slot,] destination)`

- 语法: `setDestination([slot: number,] destination: string): boolean[, string]`
- 用途: 给车票机内的金票写入目的地。
- 参数:
  - `slot: number`
    可选，`1` 到 `10` 的票槽编号。省略时使用当前选中槽位。
  - `destination: string`
    要写入的目的地名称。
- 返回值:
  - `true` 表示成功。
  - `false, "there is no golden ticket in that slot"` 表示目标槽位中没有金票。
- 说明:
  - 目的地长度不能超过 `32` 个字符。

#### `getDestination([slot])`

- 语法: `getDestination([slot: number]): string[, string]`
- 用途: 读取车票机内金票保存的目的地。
- 参数:
  - `slot: number`
    可选，`1` 到 `10` 的票槽编号。省略时使用当前选中槽位。
- 返回值:
  - 成功时返回目的地字符串。
  - 目标槽位不含金票时返回 `false, "there is no golden ticket in that slot"`。

#### `getSelectedTicket()`

- 语法: `getSelectedTicket(): number`
- 用途: 返回当前选中的票槽编号。
- 返回值:
  - `1` 到 `10` 之间的用户可见槽位编号。

#### `setSelectedTicket(slot)`

- 语法: `setSelectedTicket(slot: number): number`
- 用途: 修改当前选中的票槽。
- 参数:
  - `slot: number`
    `1` 到 `10` 之间的槽位编号。
- 返回值:
  - 新选中的槽位编号。

### locomotive_relay

`locomotive_relay` 是 Computronics 的机车中继组件。

#### `getDestination()`

- 语法: `getDestination(): string[, string]`
- 用途: 返回已绑定机车当前目的地。
- 返回值:
  - 成功时返回目的地字符串。
  - 如果无法访问机车，则返回 `nil, 错误信息`。

#### `setDestination(destination)`

- 语法: `setDestination(destination: string): boolean[, string]`
- 用途: 修改已绑定机车的目的地。
- 参数:
  - `destination: string`
    新的目的地名称。
- 返回值:
  - `true` 表示成功。
  - 如果机车第 `0` 号内部槽位中没有金票，则返回 `false, "there is no golden ticket inside the locomotive"`。
  - 若中继无法访问机车，则返回 `nil, 错误信息`。

#### `getCharge()`

- 语法: `getCharge(): number[, string]`
- 用途: 返回已绑定电力机车当前电量。
- 返回值:
  - 当前电量值。
  - 无法访问时返回 `nil, 错误信息`。

#### `getMode()`

- 语法: `getMode(): string[, string]`
- 用途: 返回机车当前运行模式。
- 返回值:
  - `running`
  - `idle`
  - `shutdown`
  - 无法访问时返回 `nil, 错误信息`。

#### `getName()`

- 语法: `getName(): string[, string]`
- 用途: 返回机车自定义名称。
- 返回值:
  - 机车名称。
  - 若没有自定义名称则返回空字符串。
  - 无法访问时返回 `nil, 错误信息`。

#### 中继访问失败情况

- `relay is not bound to a locomotive`
- `locomotive is currently not detectable`
- `relay and locomotive are in different dimensions`
- `locomotive is too far away`
- `locomotive is locked`
- `locomotive out of energy`
- `not enough energy`

### steam_turbine

`steam_turbine` 是 Computronics 为 Railcraft 蒸汽涡轮机暴露的组件。

#### `getTurbineOutput()`

- 语法: `getTurbineOutput(): number`
- 用途: 返回当前涡轮输出值。
- 返回值:
  - 当前输出值。

#### `getTurbineRotorStatus()`

- 语法: `getTurbineRotorStatus(): number`
- 用途: 返回转子剩余耐久百分比。
- 返回值:
  - `0` 到 `100` 的百分比数值。
- 说明:
  - 如果第一格没有安装转子，则返回 `0`。

## 示例

```lua
local component = require("component")

if component.isAvailable("anchor") then
  local anchor = component.anchor
  print("Anchor type:", anchor.getType())
  print("Fuel:", anchor.getFuel())
end

if component.isAvailable("launcher_track") then
  local launcher = component.launcher_track
  print("Launcher force:", launcher.getForce())
  local ok, err = launcher.setForce(20)
  print("Set force:", ok, err or "")
end

if component.isAvailable("digital_receiver_box") then
  local receiver = component.digital_receiver_box
  local event = {computer.pullSignal()}
  if event[1] == "aspect_changed" then
    print("Signal", event[2], "changed to", event[3])
  end
end
```

## 相关内容

- `buildcraft`
- `computronics`
