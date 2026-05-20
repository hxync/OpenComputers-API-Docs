# vanilla

## 概述

本页说明 OpenComputers 向 Lua 暴露的原版 Minecraft 集成接口。可用组件覆盖库存、液体处理器、液体罐、命令方块、熔炉、酿造台、音符盒、唱片机、信标、比较器以及刷怪笼。

## 可用性

- 依赖: `vanilla`
- 标签: `integration-required`

## 组件名

- `fluid_handler`
- `fluid_tank`
- `inventory`
- `command_block`
- `brewing_stand`
- `furnace`
- `beacon`
- `comparator`
- `note_block`
- `jukebox`
- `mob_spawner`

## 主要用途

- 检查和整理普通容器库存。
- 读取原版兼容液体罐的信息。
- 自动化控制命令方块、音符盒和唱片机。
- 监控熔炉、酿造台、信标、比较器和刷怪笼状态。

## 组件

### fluid_handler

`fluid_handler` 用于实现 `IFluidHandler` 的方块。

#### `getTankInfo([side])`

- 语法: `getTankInfo([side: number]): table`
- 用途: 返回指定方向可见的液体罐信息。
- 参数:
  - `side: number`
    可选 ForgeDirection 方向编号。默认使用 `ForgeDirection.UNKNOWN`，通常对应 `6`。
- 返回值:
  - 由多个液体罐信息表组成的 Lua 数组。
- 说明:
  - 适合用于有方向区别的液体机器或储罐。

### fluid_tank

`fluid_tank` 用于实现 `IFluidTank` 的方块。

#### `getInfo()`

- 语法: `getInfo(): table`
- 用途: 返回该液体罐的信息。
- 返回值:
  - 一个包含容量与当前液体内容的液体罐信息表。

### inventory

`inventory` 用于普通 Minecraft 库存，也就是实现 `IInventory` 的方块。

#### `getInventoryName()`

- 语法: `getInventoryName(): string[, string]`
- 用途: 返回库存名称。
- 返回值:
  - 成功时返回库存名称。
  - 如果伪玩家权限检查失败，则返回 `nil, "permission denied"`。

#### `getInventorySize()`

- 语法: `getInventorySize(): number[, string]`
- 用途: 返回库存槽位数量。
- 返回值:
  - 槽位总数。
  - 权限不足时返回 `nil, "permission denied"`。

#### `getSlotStackSize(slot)`

- 语法: `getSlotStackSize(slot: number): number[, string]`
- 用途: 返回指定槽位当前有多少物品。
- 参数:
  - `slot: number`
    从 `1` 开始的槽位编号。
- 返回值:
  - 当前堆叠数量。
  - 如果槽为空则返回 `0`。
  - 权限不足时返回 `nil, "permission denied"`。

#### `getSlotMaxStackSize(slot)`

- 语法: `getSlotMaxStackSize(slot: number): number[, string]`
- 用途: 返回指定槽位允许的最大堆叠数。
- 参数:
  - `slot: number`
    从 `1` 开始的槽位编号。
- 返回值:
  - 槽位内物品最大堆叠数与库存上限两者较小值。
  - 如果槽为空，则返回库存自身上限。
  - 权限不足时返回 `nil, "permission denied"`。

#### `compareStacks(slotA, slotB)`

- 语法: `compareStacks(slotA: number, slotB: number): boolean[, string]`
- 用途: 比较两个槽位中的物品是否相同。
- 参数:
  - `slotA: number`
    第一个从 `1` 开始的槽位编号。
  - `slotB: number`
    第二个从 `1` 开始的槽位编号。
- 返回值:
  - 如果两个槽都为空、两个槽其实是同一槽，或两边物品一致，则返回 `true`。
  - 否则返回 `false`。
  - 权限不足时返回 `nil, "permission denied"`。
- 说明:
  - 比较规则是物品类型一致，且对有子类型的物品还要求 damage 值一致。

#### `transferStack(slotA, slotB[, count])`

- 语法: `transferStack(slotA: number, slotB: number[, count: number]): boolean[, string]`
- 用途: 在同一个库存内部移动、合并或交换两个槽位中的物品。
- 参数:
  - `slotA: number`
    来源槽位，编号从 `1` 开始。
  - `slotB: number`
    目标槽位，编号从 `1` 开始。
  - `count: number`
    可选移动上限，默认 `64`，随后会再限制到库存最大堆叠上限。
- 返回值:
  - 成功执行操作或无需实际移动时返回 `true`。
  - 无法完成时返回 `false`。
  - 权限不足时返回 `nil, "permission denied"`。
- 说明:
  - 目标槽为空时会直接搬运物品。
  - 两边物品相同时会尝试合并堆叠。
  - 两边物品不同但 `count` 足够覆盖整个源堆叠时，会执行交换。

#### `getStackInSlot(slot)`

- 语法: `getStackInSlot(slot: number): table[, string]`
- 用途: 返回指定槽位的完整物品栈描述。
- 参数:
  - `slot: number`
    从 `1` 开始的槽位编号。
- 返回值:
  - 成功时返回物品栈描述表。
  - 若配置未启用物品栈检查，则返回 `nil, "not enabled in config"`。
  - 权限不足时返回 `nil, "permission denied"`。

#### `getAllStacks()`

- 语法: `getAllStacks(): table[, string]`
- 用途: 返回整个库存中所有槽位的物品栈描述。
- 返回值:
  - 物品栈描述表数组。
  - 若配置未启用物品栈检查，则返回 `nil, "not enabled in config"`。
  - 权限不足时返回 `nil, "permission denied"`。

### command_block

`command_block` 用于原版命令方块。

#### `getCommand()`

- 语法: `getCommand(): string`
- 用途: 返回当前保存的命令内容。
- 返回值:
  - 命令字符串。

#### `setCommand(value)`

- 语法: `setCommand(value: string): boolean`
- 用途: 替换命令方块当前保存的命令。
- 参数:
  - `value: string`
    新命令文本。
- 返回值:
  - `true` 表示成功。

#### `executeCommand()`

- 语法: `executeCommand(): number[, string]`
- 用途: 执行当前保存的命令。
- 返回值:
  - 返回命令成功计数和状态文本。
  - 如果服务器关闭了命令方块，则返回 `nil, "command blocks are disabled"`。
- 说明:
  - 这个调用会先短暂停顿一下，让命令方块状态先完成更新。

### brewing_stand

`brewing_stand` 用于原版酿造台。

#### `getBrewTime()`

- 语法: `getBrewTime(): number`
- 用途: 返回当前酿造还剩多少 tick。
- 返回值:
  - 剩余酿造时间。

### furnace

`furnace` 用于原版熔炉。

#### `getBurnTime()`

- 语法: `getBurnTime(): number`
- 用途: 返回上一次消耗的燃料还剩多少燃烧 tick。
- 返回值:
  - 剩余燃烧时间。

#### `getCookTime()`

- 语法: `getCookTime(): number`
- 用途: 返回当前物品已经烧炼了多少 tick。
- 返回值:
  - 当前烧炼进度。

#### `getCurrentItemBurnTime()`

- 语法: `getCurrentItemBurnTime(): number`
- 用途: 返回当前燃料总共可以燃烧多少 tick。
- 返回值:
  - 当前燃料完整燃烧时长。

#### `isBurning()`

- 语法: `isBurning(): boolean`
- 用途: 返回熔炉当前是否正在工作。
- 返回值:
  - `true` 表示正在燃烧。
  - `false` 表示未燃烧。

### beacon

`beacon` 用于原版信标。

#### `getLevels()`

- 语法: `getLevels(): number`
- 用途: 返回信标底座层级数。
- 返回值:
  - 当前完整层数。

#### `getPrimaryEffect()`

- 语法: `getPrimaryEffect(): string`
- 用途: 返回当前主效果名称。
- 返回值:
  - 药水效果名称字符串。
  - 如果当前没有有效主效果，则返回 `nil`。

#### `getSecondaryEffect()`

- 语法: `getSecondaryEffect(): string`
- 用途: 返回当前副效果名称。
- 返回值:
  - 药水效果名称字符串。
  - 如果当前没有有效副效果，则返回 `nil`。

### comparator

`comparator` 用于比较器方块实体。

#### `getOutputSignal()`

- 语法: `getOutputSignal(): number`
- 用途: 返回比较器当前输出强度。
- 返回值:
  - 当前红石信号等级。

### note_block

`note_block` 用于音符盒。

#### `getPitch()`

- 语法: `getPitch(): number`
- 用途: 返回当前设置的音高。
- 返回值:
  - `1` 到 `25` 之间的音高值。

#### `setPitch(value)`

- 语法: `setPitch(value: number): boolean`
- 用途: 设置音符盒音高。
- 参数:
  - `value: number`
    `1` 到 `25` 之间的音高值。
- 返回值:
  - `true` 表示成功。
- 说明:
  - 非法音高会触发参数错误。

#### `trigger([pitch])`

- 语法: `trigger([pitch: number]): boolean`
- 用途: 立即触发音符盒播放，并且可以顺便先改音高。
- 参数:
  - `pitch: number`
    可选音高值，范围 `1` 到 `25`。
- 返回值:
  - 如果上方是空气、可以真正发声，则返回 `true`。
  - 如果上方被挡住，则返回 `false`。
- 说明:
  - 即使返回 `false`，只要传入了 `pitch`，音高依然会先被更新。

### jukebox

`jukebox` 用于唱片机方块实体。

#### `getRecord()`

- 语法: `getRecord(): string`
- 用途: 返回当前插入唱片的标题。
- 返回值:
  - 本地化唱片标题。
  - 如果没有有效唱片，则返回 `nil`。

#### `play()`

- 语法: `play(): boolean`
- 用途: 开始播放当前插入的唱片。
- 返回值:
  - 找到有效唱片并成功播放时返回 `true`。
  - 如果没有有效唱片，则返回 `nil`。

#### `stop()`

- 语法: `stop()`
- 用途: 停止播放当前唱片。
- 返回值:
  - 无返回值。

### mob_spawner

`mob_spawner` 用于原版刷怪笼。

#### `getSpawningMobName()`

- 语法: `getSpawningMobName(): string`
- 用途: 返回刷怪笼当前设置的实体 id。
- 返回值:
  - 实体名称字符串。

## 示例

```lua
local component = require("component")

if component.isAvailable("inventory") then
  local inv = component.inventory
  print("Inventory:", inv.getInventoryName(), inv.getInventorySize())
end

if component.isAvailable("furnace") then
  local furnace = component.furnace
  print("Burning:", furnace.isBurning(), furnace.getCookTime())
end

if component.isAvailable("note_block") then
  local note = component.note_block
  note.setPitch(13)
  note.trigger()
end
```

## 相关内容

- `redstone`
- `cofh`
