# opencomputers-upgrade-navigation

## 概述

本页说明导航升级暴露出的 `navigation` 组件接口。它用于让机器人、无人机、平板等宿主读取相对位置、朝向、扫描范围，以及附近的 waypoint 信息。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`navigation`
- 常见宿主：机器人、无人机、平板，以及其他支持导航升级的可旋转宿主

## 用途

- 获取宿主在导航地图中的相对坐标。
- 获取宿主当前朝向。
- 获取导航升级本身的有效工作半径。
- 扫描附近的 waypoint 方块并读取其标签、地址和红石输入强度。

## 位置模型

导航升级内部会维护一个地图中心点。`getPosition()` 返回的是相对于这个中心点的坐标，而不是世界绝对坐标。

这意味着：

- 同一设备移动后，返回值会随位置变化。
- 如果设备已经离开导航升级可映射的区域，`getPosition()` 会失败并返回 `nil, "out of range"`。

## API

### getPosition

- 语法：`navigation.getPosition()`
- 返回值：
  - 成功：`relativeX, relativeY, relativeZ`
  - 失败：`nil, "out of range"`
- 用途：返回宿主当前在导航地图中的相对坐标。
- 说明：
  - 返回值不是世界绝对坐标。
  - 超出映射范围时无法获得位置。

### getFacing

- 语法：`navigation.getFacing()`
- 返回值：`number`
- 用途：返回宿主当前朝向。
- 说明：
  - 返回值使用 Forge 方向枚举的序号。
  - 对机器人和其他可旋转设备来说，这个值可用于判断前后左右方向。

### getRange

- 语法：`navigation.getRange()`
- 返回值：`number`
- 用途：返回导航升级的有效工作半径。
- 说明：
  - 该范围决定了位置映射和 waypoint 扫描的有效区域。

### findWaypoints

- 语法：`navigation.findWaypoints(range)`
- 参数：
  - `range:number`：希望扫描的半径。
- 返回值：
  - 成功：`table`
  - 失败：`nil, "not enough energy"`
- 用途：扫描指定半径内的 waypoint。
- 行为：
  - 请求半径会被限制在组件允许的最大扫描范围内。
  - 当 `range <= 0` 时，会直接返回空表。
  - 扫描会消耗能量，并可能让当前协程短暂停顿。

返回结果中的每一项都是一个表，包含：

- `position`：`{dx, dy, dz}`，相对于宿主当前位置的偏移量。
- `redstone`：该 waypoint 当前接收到的红石强度。
- `label`：waypoint 上设置的标签文本。
- `address`：该 waypoint 的组件地址。

位置有一个常见细节：

- `position` 指向的是 waypoint 正前方可到达的那个方块。
- 它不是 waypoint 方块自身所占据的位置。

## 使用示例

```lua
local component = require("component")
local navigation = component.navigation

local x, y, z = navigation.getPosition()
if x then
  print("Relative position:", x, y, z)
else
  print("Position is out of range.")
end

print("Facing:", navigation.getFacing())
print("Range:", navigation.getRange())

local waypoints, reason = navigation.findWaypoints(32)
if not waypoints then
  error(reason)
end

for i, waypoint in ipairs(waypoints) do
  print(i, waypoint.label, table.unpack(waypoint.position))
end
```

### 示例用途

适合实现自动导航、路径标记、回家点搜寻、区域巡航和 waypoint 定位菜单。

## 相关内容

- `waypoint`
- `robot`
- `tablet`
