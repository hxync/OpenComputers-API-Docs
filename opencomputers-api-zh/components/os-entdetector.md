# os_entdetector

## 概述

`os_entdetector` 组件用于控制 OpenSecurity 实体探测器。它可以返回探测器自身坐标、只扫描玩家，或者扫描范围内的全部实体，并为每个匹配目标发送一条 `entityDetect` 信号。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_entdetector`
- 常见宿主：放置在世界中的 OpenSecurity 实体探测器

## 用法

```lua
local component = require("component")
local detector = component.os_entdetector

if not detector then
  error("未安装 os_entdetector 组件")
end
```

## 范围与能量

- 最大扫描范围来自 `OpenSecurity.rfidRange` 配置。
- 配置默认值为 `16`，允许的配置范围是 `1` 到 `64`。
- 回调会先把请求范围限制在该最大值以内。
- 限制完成后，源码还会再把这个值除以 `2`，并把这个减半后的数值作为真正的扫描半径。
- 能量消耗为 `5 * 实际半径`。

示例：

- 请求范围 `16`
- 实际半径 `8`
- 能量消耗 `40`

## 信号

每检测到一个目标，组件都会发送：

```lua
computer.signal("entityDetect", name, range, x, y, z)
```

载荷说明：

- `name`：玩家显示名或实体命令发送者名称
- `range`：目标到探测器方块的距离
- `x`、`y`、`z`：
  - 当 `offset` 为 `false` 时，返回世界绝对坐标
  - 当 `offset` 为 `true` 时，返回相对于探测器的偏移坐标

## 接口

### greet

- 语法：`detector.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

### getLoc

- 语法：`detector.getLoc()`
- 返回：`x, y, z`
- 用途：返回探测器方块坐标。

示例：

```lua
local x, y, z = detector.getLoc()
print(x, y, z)
```

### scanPlayers

- 语法：`detector.scanPlayers([range[, offset]])`
- 返回：
  - 成功：`table`
  - 失败：`false, reason`
- 用途：扫描范围内的玩家实体。

参数说明：

- `range:number` 可选：请求的扫描半径，内部还会再减半后实际使用。
- `offset:boolean` 可选：是否把返回坐标改为相对探测器的偏移值。

返回表条目格式：

- `entry.name`
- `entry.range`
- `entry.x`
- `entry.y`
- `entry.z`

行为说明：

- 这里只会返回多人游戏玩家实体。
- 没有目标时会返回空表。

示例：

```lua
local results, reason = detector.scanPlayers(16, true)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.x, entry.y, entry.z)
end
```

### scanEntities

- 语法：`detector.scanEntities([range[, offset]])`
- 返回：
  - 成功：`table`
  - 失败：`false, reason`
- 用途：扫描范围内的全部实体并返回结果。

参数说明：

- `range:number` 可选：请求的扫描半径，内部还会再减半后实际使用。
- `offset:boolean` 可选：是否把返回坐标改为相对探测器的偏移值。

行为说明：

- 虽然旧注释写的是“排除玩家”，但实际源码这里并不会排除玩家。
- 玩家和非玩家实体都会被返回。
- 没有目标时会返回空表。

示例：

```lua
local results, reason = detector.scanEntities(12, false)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.range)
end
```

## 示例

```lua
local event = require("event")

local results = assert(detector.scanPlayers(16, true))
for _, entry in pairs(results) do
  print("玩家：", entry.name, entry.x, entry.y, entry.z)
end

while true do
  local _, _, _, name, range, x, y, z = event.pull("computer.signal")
  if name == "entityDetect" then
    print("检测到目标：", range, x, y, z)
  end
end
```

## 说明

- 探测器在扫描时会切换方块元数据状态，这只是视觉反馈，不影响 Lua 回调的使用方式。
- 如果 OC 网络能量不足，这两个扫描回调都会返回 `false, "Not enough power in OC Network."`。

## 相关内容

- `component`
- `os_rfidreader`
