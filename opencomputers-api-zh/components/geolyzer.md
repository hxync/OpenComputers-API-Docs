# geolyzer

## 概述

`geolyzer` 组件用于分析附近地形和相邻方块，常见用途包括找矿、地层采样、根据方块属性驱动自动化逻辑，以及读取 GregTech 地下流体信息。

和普通的方块查询不同，地质分析仪的大多数操作都会消耗能量；当内部缓冲不足时，调用会失败。较大的区域扫描还会受到扫描范围和总体积限制。

## 可用性

- 仓库：`opencomputers`
- 组件名：`geolyzer`
- 常见宿主：安装了对应硬件的电脑、机器人、平板、无人机、微控制器

## 用法

```lua
local component = require("component")
local geolyzer = component.geolyzer

if not geolyzer then
  error("未安装 geolyzer 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### analyze

- 语法：`geolyzer.analyze(side[, options])`
- 返回值：
  - 成功：`table`
  - 失败：`nil, reason`
- 用途：读取宿主指定方向上、紧邻方块的扩展信息。

参数说明：

- `side:number`：要分析的方向。
- `options:table` 可选：传递给事件系统的附加分析选项。大多数脚本保持为空即可。

说明：

- 只有在 OpenComputers 配置中启用了物品栈检查时，此调用才可用。
- 该调用会消耗地质分析仪扫描能量。
- 可能返回以下失败原因：
  - `"not enabled in config"`
  - `"not enough energy"`
  - `"scan was canceled"`

示例：

```lua
local sides = require("sides")
local data, reason = geolyzer.analyze(sides.front)
if not data then
  io.stderr:write("分析失败: " .. tostring(reason) .. "\n")
else
  print("方块名:", data.name or "未知")
  print("硬度:", data.hardness or "无")
end
```

### canSeeSky

- 语法：`geolyzer.canSeeSky()`
- 返回值：`boolean`
- 用途：检查宿主正上方的方块空间是否与天空直通，没有被任何方块遮挡。

它适合用来做室外检测、太阳能前置判断、天气相关自动化逻辑。

示例：

```lua
if geolyzer.canSeeSky() then
  print("设备上方直通天空")
else
  print("设备上方被遮挡")
end
```

### isSunVisible

- 语法：`geolyzer.isSunVisible()`
- 返回值：`boolean`
- 用途：判断当前时刻太阳是否真正照射到宿主上方。

它比 `canSeeSky()` 更严格，除了上方不能被遮挡外，还会受昼夜和天气影响。沙漠群系在部分天气条件下的表现也会和其他群系不同。

示例：

```lua
if geolyzer.isSunVisible() then
  print("当前能接收到直射阳光")
else
  print("当前没有直射阳光")
end
```

### scan

- 语法：`geolyzer.scan(x, z[, y, w, d, h][, ignoreReplaceable|options])`
- 返回值：
  - 成功：`table`
  - 失败：`nil, reason`
- 用途：对宿主相对坐标处的一条竖直柱，或一个受限的三维区域进行密度采样。

参数说明：

- `x:number`：相对 X 偏移。
- `z:number`：相对 Z 偏移。
- `y:number` 可选：三维扫描时的起始相对 Y 偏移。
- `w:number` 可选：X 方向宽度。
- `d:number` 可选：Z 方向深度。
- `h:number` 可选：Y 方向高度。
- `ignoreReplaceable:boolean` 可选：兼容旧写法。传入 `true` 时，会在结果中过滤可替换方块。
- `options:table` 可选：高级扫描选项。

行为说明：

- 只传 `x` 和 `z` 时，会扫描一条竖直柱，范围固定为 `y = -32` 到 `y = 31`。
- 传入 `y, w, d, h` 时，总扫描体积不能超过 `64` 个方块。
- 请求区域内每个坐标都必须在配置允许的地质分析仪范围内。
- 该调用会消耗能量，并可能返回：
  - `"not enough energy"`
  - `"scan was canceled"`
- 非法请求会直接抛出错误，例如：
  - `"volume too large (maximum is 64)"`
  - `"location out of bounds"`

示例：扫描设备正下方和正上方的一整列数据。

```lua
local densities, reason = geolyzer.scan(0, 0)
if not densities then
  io.stderr:write("扫描失败: " .. tostring(reason) .. "\n")
else
  for y, hardness in ipairs(densities) do
    print(y - 33, hardness)
  end
end
```

示例：从设备下方 8 格开始，扫描一个 `4 x 4 x 4` 立方体。

```lua
local data = assert(({geolyzer.scan(-1, -1, -8, 4, 4, 4)})[1])
print("扫描到的方块数:", #data)
```

### scanUndergroundFluids

- 语法：`geolyzer.scanUndergroundFluids()`
- 返回值：`table`
- 用途：读取当前区块的 GregTech 地下流体信息。

返回表通常包含：

- `type:string`：本地化后的流体名称。
- `quantity:number`：当前区块报告的储量。

只有在整合包启用了 OpenComputers 对 GregTech 地下流体系统的兼容时，这个接口才有实际意义。

示例：

```lua
local fluid = geolyzer.scanUndergroundFluids()
print("流体类型:", fluid.type)
print("储量:", fluid.quantity)
```

### store

- 语法：`geolyzer.store(side, dbAddress, dbSlot)`
- 返回值：
  - 成功：`boolean`
  - 失败：`nil, reason`
- 用途：将相邻方块对应的物品表示写入数据库组件的指定槽位。

参数说明：

- `side:number`：要读取的方块方向。
- `dbAddress:string`：目标数据库组件地址。
- `dbSlot:number`：数据库中的目标槽位。

返回值说明：

- 成功时，布尔值表示目标槽位原来是否已经有物品。
- 失败时，常见原因包括：
  - `"not enough energy"`
  - `"block has no registered item representation"`

示例：

```lua
local database = component.database
local sides = require("sides")

local replaced, reason = geolyzer.store(sides.down, database.address, 1)
if replaced == nil then
  io.stderr:write("写入数据库失败: " .. tostring(reason) .. "\n")
else
  print("是否替换了原有物品:", replaced)
end
```

## 相关内容

- `component`
- `database`
- `robot`
- `tablet`
