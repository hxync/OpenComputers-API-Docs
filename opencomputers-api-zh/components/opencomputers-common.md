# opencomputers-common

## 概述

这个文件名本身具有误导性：Lua 里并不存在叫做 `opencomputers-common` 的真实组件。

本页用于整理一个历史遗留的混合导出页，它实际上把两个真实组件的接口混在了一起：

- `configurator`
- `inventory_controller`

## 本页实际覆盖的组件

### configurator

`configurator` 组件用于检查和配置相邻的 Ender IO 导管。

- 常见宿主：适配器、无人机、机器人、微控制器等具备配置器环境的宿主
- 实际 Lua 组件名：`configurator`
- 主要用途：查看导管状态、修改导管设置

### inventory_controller

本页里还有一部分“机器人升级槽辅助接口”，它们实际上属于 `inventory_controller`，并不是 `configurator`。

- 常见宿主：机器人
- 实际 Lua 组件名：`inventory_controller`
- 这里涉及的主要用途：交换工具和已安装升级

## configurator 使用前提

本页中大多数 `configurator` 相关接口都要求满足以下条件：

- 必须安装 Ender IO。
- 目标相邻方块必须是 Ender IO 导管束。
- 如果要修改物品过滤器，对应导管上必须已经装有兼容的过滤器升级。

常见失败返回：

- `false, "EnderIO not loaded"`
- `nil, "No conduit here"`
- `nil, "no item conduit here"`
- `nil, "no liquid conduit here"`
- `nil, "no ender conduit here"`

颜色名或模式名如果写错，通常会直接抛出错误，而不是返回整洁的失败结果，所以必须传入受支持的取值。

## configurator 参数约定

导管相关接口普遍使用两个方向参数：

- `side:number`：宿主的哪一侧紧邻着目标导管束方块。
- `conduitDirection:number`：要检查或配置的是该导管束的哪一个面。

例如：

- 对机器人来说，`side` 按机器人朝向解释。
- 对适配器来说，`side` 就是世界坐标中的相邻方块方向。

## 支持的取值

### 染料颜色

颜色参数会先转成大写再匹配，因此推荐使用下面这些标准名称：

- `black`
- `red`
- `green`
- `brown`
- `blue`
- `purple`
- `cyan`
- `light_gray`
- `gray`
- `pink`
- `lime`
- `yellow`
- `light_blue`
- `magenta`
- `orange`
- `white`

### 连接模式

导管连接模式支持：

- `IN_OUT`
- `INPUT`
- `OUTPUT`
- `DISABLED`
- `NOT_SET`

### 提取红石模式

提取红石模式支持：

- `IGNORE`
- `ON`
- `OFF`
- `NEVER`

## configurator API

### getConduitConfiguration

- 语法：`configurator.getConduitConfiguration(side, conduitDirection)`
- 参数：
  - `side:number`：相邻导管束所在方向。
  - `conduitDirection:number`：目标导管束上要查看的那个面。
- 返回：
  - 成功时：`table`
  - 失败时：`nil, "No conduit here"` 或 `nil, "EnderIO not loaded"`
- 用途：返回指定方向导管束的结构化配置快照。

顶层返回键可能包括：

- `HasFacade:boolean`
- `ItemConduit:table`，当存在物品导管时出现
- `LiquidConduit:table`，当存在流体导管时出现
- `RedstoneConduit:table`，当存在红石导管时出现

`ItemConduit` 可能包含的键：

- `OutputColor`
- `InputColor`
- `RoundRobinEnabled`
- `SelfFeedEnabled`
- `ExtractionRedstoneConditionMet`
- `OutputPriority`
- `InputFilter`
- `OutputFilter`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

如果是末影流体导管，`LiquidConduit` 可能包含：

- `InputFilter`
- `OutputFilter`
- `InputColor`
- `OutputColor`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

如果是普通储罐导管，`LiquidConduit` 可能包含：

- `FluidType`
- `ConnectionMode`
- `ExtractionRedstoneMode`
- `ExtractionRedstoneColor`

`RedstoneConduit` 可能包含：

- `SignalColor`
- `IsOutputStrong`
- `ConnectionMode`

物品过滤器表可能包含：

- `Advanced`
- `Blacklist`
- `MatchMeta`
- `MatchNBT`
- `UseOreDict`
- `Sticky`
- `FuzzyMode`
- `FilterItems`

流体过滤器表可能包含：

- `blacklist`
- `"1"` 到 `"5"`，分别表示各过滤槽里的流体名称

### setItemConduitColor

- 语法：`configurator.setItemConduitColor(side, conduitDirection, isInput, color)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`：`true` 表示输入颜色，`false` 表示输出颜色。
  - `color:string`：使用上面列出的染料颜色之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：设置物品导管该面的输入色或输出色。

### setItemConduitOutputPriority

- 语法：`configurator.setItemConduitOutputPriority(side, conduitDirection, priority)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `priority:number`：该输出面的优先级。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：设置物品导管该面的输出优先级。

### setItemConduitFilter

- 语法：`configurator.setItemConduitFilter(side, conduitDirection, databaseAddress, dbSlot, filterIndex, isInput)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `databaseAddress:string`：同网络中数据库组件的地址。
  - `dbSlot:number`：数据库中要复制的槽位。
  - `filterIndex:number`：导管过滤器内部目标槽位。这个索引会直接传入底层，因此实际按 `0` 开始理解更安全。
  - `isInput:boolean`：`true` 设置输入过滤器，`false` 设置输出过滤器。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"`、`false, "Wrong or no item filter"`，或数据库/导管相关错误
- 用途：把数据库中的一个物品样本复制到物品导管过滤器的指定槽位。

使用说明：

- 导管上必须已经装有可直接写槽位的普通物品过滤器。
- `dbSlot` 仍然遵循 OpenComputers 的常规槽位校验规则。

### setItemConduitConnectionMode

- 语法：`configurator.setItemConduitConnectionMode(side, conduitDirection, mode)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`：`IN_OUT`、`INPUT`、`OUTPUT`、`DISABLED`、`NOT_SET` 之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：设置该面物品导管的连接模式。

### setItemConduitExtractionRedstoneColor

- 语法：`configurator.setItemConduitExtractionRedstoneColor(side, conduitDirection, color)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `color:string`：支持的染料颜色之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：设置该面物品导管提取红石所使用的颜色通道。

### setItemConduitExtractionRedstoneMode

- 语法：`configurator.setItemConduitExtractionRedstoneMode(side, conduitDirection, mode)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`：`IGNORE`、`ON`、`OFF`、`NEVER` 之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：设置该面物品导管在何种红石条件下允许提取。

### setEnderLiquidConduitFilter

- 语法：`configurator.setEnderLiquidConduitFilter(side, conduitDirection, fluid, blacklist, isInput[, filterIndex])`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `fluid:string`：已注册的流体名，例如 `water`、`lava`。
  - `blacklist:boolean`：`true` 表示黑名单，`false` 表示白名单。
  - `isInput:boolean`：`true` 修改输入过滤器，`false` 修改输出过滤器。
  - `filterIndex:number`：可选，过滤槽索引，默认 `0`。末影流体过滤器共有五个槽位，索引范围为 `0` 到 `4`。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no ender conduit here"`
- 用途：替换末影流体导管中的一个过滤项。

### setEnderLiquidConduitColor

- 语法：`configurator.setEnderLiquidConduitColor(side, conduitDirection, isInput, color)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`
  - `color:string`
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no ender conduit here"`
- 用途：设置末影流体导管该面的输入色或输出色。

### setLiquidConduitConnectionMode

- 语法：`configurator.setLiquidConduitConnectionMode(side, conduitDirection, mode)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`：`IN_OUT`、`INPUT`、`OUTPUT`、`DISABLED`、`NOT_SET` 之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no liquid conduit here"`
- 用途：设置流体导管该面的连接模式。

### setLiquidConduitExtractionRedstoneColor

- 语法：`configurator.setLiquidConduitExtractionRedstoneColor(side, conduitDirection, color)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `color:string`
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no liquid conduit here"`
- 用途：设置流体导管该面的提取红石颜色。

### setLiquidConduitExtractionRedstoneMode

- 语法：`configurator.setLiquidConduitExtractionRedstoneMode(side, conduitDirection, mode)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `mode:string`：`IGNORE`、`ON`、`OFF`、`NEVER` 之一。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no liquid conduit here"`
- 用途：设置流体导管该面的提取红石模式。

### setRedstoneConduitSignalColor

- 语法：`configurator.setRedstoneConduitSignalColor(side, conduitDirection, color)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `color:string`
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no liquid conduit here"`
- 用途：设置绝缘红石导管连接所使用的颜色通道。

### setRedstoneConduitSignalStrength

- 语法：`configurator.setRedstoneConduitSignalStrength(side, conduitDirection, strength)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `strength:boolean`：`true` 为强信号，`false` 为弱信号。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no liquid conduit here"`
- 用途：设置绝缘红石导管输出的是强红石还是弱红石。

### replaceConduitFilter

- 语法：`configurator.replaceConduitFilter(side, conduitDirection, isInput)`
- 参数：
  - `side:number`
  - `conduitDirection:number`
  - `isInput:boolean`：`true` 表示输入过滤器，`false` 表示输出过滤器。
- 返回：
  - 成功时：`true`
  - 失败时：`false, "EnderIO not loaded"` 或 `nil, "no item conduit here"`
- 用途：机器人专用辅助接口，用机器人当前选中槽中的物品和导管过滤器升级进行交换。

行为细节：

- 只有机器人版 `configurator` 才暴露这个接口。
- 被替换下来的旧过滤器物品会进入机器人当前选中槽。

## 机器人上的 inventory_controller 接口

下面这些接口属于真实的 `inventory_controller` 组件，不属于 `configurator`。

### equip

- 语法：`inventoryController.equip()`
- 返回：`boolean`
- 用途：把机器人已装备工具槽与当前选中的内部库存槽进行交换。

### installUpgrade

- 语法：`inventoryController.installUpgrade([slot])`
- 参数：
  - `slot:number`：可选，机器人容器槽位编号，默认是 `1`。
- 返回：
  - 成功时：`true`
  - 失败时：`false`、`false, "not a container slot"`、或 `false, "Invalid upgrade"`
- 用途：把指定机器人升级容器槽中的升级，与当前选中库存槽中的物品进行交换。

### getUpgradeContainerType

- 语法：`inventoryController.getUpgradeContainerType(slot)`
- 参数：
  - `slot:number`：机器人容器槽位编号。
- 返回：`string`
- 用途：返回该机器人槽位允许安装的容器类型。

行为细节：

- 如果该槽位根本不是容器槽，会返回 `"None"`。

### getUpgradeContainerTier

- 语法：`inventoryController.getUpgradeContainerTier(slot)`
- 参数：
  - `slot:number`：机器人容器槽位编号。
- 返回：`number`
- 用途：返回该机器人容器槽位的等级要求。

行为细节：

- 如果该槽位不是容器槽，会返回 `0`。

## 示例

### 示例：读取相邻导管配置

```lua
local component = require("component")
local sides = require("sides")

local configurator = component.configurator
local info = configurator.getConduitConfiguration(sides.front, sides.up)

print("是否有外观板：", info.HasFacade)
if info.ItemConduit then
  print("物品连接模式：", tostring(info.ItemConduit.ConnectionMode))
  print("输出优先级：", info.ItemConduit.OutputPriority)
end
```

### 示例：给机器人安装升级

```lua
local component = require("component")
local robot = require("robot")

local inventoryController = component.inventory_controller

robot.select(1)
local ok, err = inventoryController.installUpgrade(1)
print("升级安装结果：", ok, err)
```

## 相关内容

- `opencomputers-inventory-control`
- `robot`
- `adapter`
