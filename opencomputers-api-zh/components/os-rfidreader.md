# os_rfidreader

## 概述

`os_rfidreader` 组件用于扫描附近携带 RFID 数据的目标，并且同时通过返回表和 `rfidData` 信号汇报结果。OpenSecurity 对这个组件提供了两种形态：

- 放置在世界中的 RFID 读卡器方块
- RFID Reader Card 升级卡

这两种形态使用同一个组件名，回调行为也基本一致。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_rfidreader`
- 常见宿主：
  - 放置在世界中的 OpenSecurity RFID 读卡器
  - 装入受支持设备中的 OpenSecurity RFID Reader Card，例如平板

## 用法

```lua
local component = require("component")
local reader = component.os_rfidreader

if not reader then
  error("未安装 os_rfidreader 组件")
end
```

## 会扫描哪些对象

扫描逻辑会从以下三类位置寻找 RFID 数据：

- 玩家背包中的 RFID 卡
- OpenComputers 无人机背包中的 RFID 卡
- 实体 NBT 中 `entityData.rfidData` 结构里的数据

每找到一个 RFID 载荷，就会发送：

```lua
computer.signal("rfidData", name, range, data, uuid)
```

信号字段说明：

- `name`：玩家显示名、无人机或实体名称
- `range`：目标与宿主设备之间的距离
- `data`：RFID 负载字符串
- `uuid`：
  - 默认返回真实 UUID
  - 若启用 `ignoreUUIDs`，则返回 `"-1"`

扫描返回表中还会额外包含：

- `locked:boolean`

## 范围与能量

- 请求范围会先被限制到 `OpenSecurity.rfidRange`。
- 配置默认最大值是 `16`，配置允许范围为 `1` 到 `64`。
- 限制之后，源码还会再把范围除以 `2`。
- 能量消耗为 `5 * 实际半径`。

方块版与卡片版差异：

- 放置式读卡器在能量不足时返回 `false, "Not enough power in OC Network."`
- 卡片驱动在能量不足时会直接抛出同样文本的异常

## 接口

### greet

- 语法：`reader.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置问候语。

### scan

- 语法：`reader.scan([range])`
- 返回：
  - 成功：`table`
  - 失败：
    - 方块版：`false, reason`
    - 卡片版：抛出异常
- 用途：扫描周围区域中的 RFID 数据。

参数说明：

- `range:number` 可选：请求半径，内部会先限制再减半。

返回表条目格式：

- `entry.name`
- `entry.range`
- `entry.data`
- `entry.uuid`
- `entry.locked`

行为说明：

- 每找到一个 RFID 载荷，就会生成一个返回表条目。
- 同一个玩家或无人机如果携带多张 RFID 卡，会对应多条结果。
- 未找到任何目标时返回空表。

示例：

```lua
local results, reason = reader.scan(16)
if results == false then
  error(reason)
end

for _, entry in pairs(results) do
  print(entry.name, entry.data, entry.uuid, entry.locked)
end
```

## 示例

```lua
local event = require("event")

local results = reader.scan(16)
if results == false then
  error("scan failed")
end

for _, entry in pairs(results) do
  print("发现 RFID：", entry.name, entry.data)
end

while true do
  local _, _, _, eventName, name, range, data, uuid = event.pull("computer.signal")
  if eventName == "rfidData" then
    print(name, range, data, uuid)
  end
end
```

## 相关内容

- `component`
- `os_magreader`
- `os_cardwriter`
