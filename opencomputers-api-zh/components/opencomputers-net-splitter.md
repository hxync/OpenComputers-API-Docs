# opencomputers-net-splitter

## 概述

本页说明 `net_splitter` 组件。网络分流器是一种可以分别打开或关闭六个面的网络方块，用来把 OpenComputers 线缆网络按方向拆分成多个独立段。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`net_splitter`
- 对应实现：`li.cil.oc.common.tileentity.NetSplitter`
- 常见宿主：已放置的网络分流器方块

## 作用

- 分别控制每个面的网络连通状态。
- 读取当前六个面的实际开启或关闭状态。
- 一次调用同时修改全部六个面的状态。

## 方向编号

使用 OpenComputers 常见的 `0` 到 `5` 方向编号。

- `0`：下
- `1`：上
- `2`：北
- `3`：南
- `4`：西
- `5`：东

建议通过 `require("sides")` 使用方向常量，而不是手写数字。

## 红石反转

当该方块收到红石信号时，会进入反转模式。

- 反转模式会把内部保存的开关状态取反后再对外生效。
- `getSides()`、`open()`、`close()`、`setSides()` 返回和操作的都是你当前实际看到的逻辑状态。
- 也就是说，即使处于反转模式，`open(side)` 依然表示“让这个面逻辑上处于打开状态”。

## API

### getSides

- 语法：`netSplitter.getSides()`
- 返回：`table`
- 用途：返回所有方向当前的实际状态。

返回结构：

- 返回表以方向编号为键。
- 值为 `true` 表示打开，`false` 表示关闭。

### setSides

- 语法：`netSplitter.setSides(settings)`
- 参数：
  - `settings:table`：以方向编号为键、布尔值为内容的表。
- 返回：`table`
- 用途：一次性设置所有方向状态，并返回修改前的实际状态表。

行为细节：

- 未提供的方向会按 `false` 处理。
- 因此，省略的方向会被关闭。
- 非布尔值也会按 `false` 处理。

### open

- 语法：`netSplitter.open(side)`
- 参数：
  - `side:number`：要打开的方向。
- 返回：
  - 成功时：`boolean`
  - 失败时：`nil, "invalid direction"`
- 用途：打开一个方向，并告知实际状态是否发生了变化。

返回含义：

- `true`：该方向原本是关闭的，这次成功变为打开。
- `false`：该方向本来就已经处于实际打开状态。

### close

- 语法：`netSplitter.close(side)`
- 参数：
  - `side:number`：要关闭的方向。
- 返回：
  - 成功时：`boolean`
  - 失败时：`nil, "invalid direction"`
- 用途：关闭一个方向，并告知实际状态是否发生了变化。

返回含义：

- `true`：该方向原本是打开的，这次成功变为关闭。
- `false`：该方向本来就已经处于实际关闭状态。

## 示例

```lua
local component = require("component")
local sides = require("sides")

local netSplitter = component.net_splitter

print("初始方向状态：")
for side, isOpen in pairs(netSplitter.getSides()) do
  print(side, isOpen)
end

local changed = netSplitter.close(sides.north)
print("北侧是否发生变化：", changed)

local previous = netSplitter.setSides({
  [sides.top] = true,
  [sides.bottom] = true,
  [sides.north] = false,
  [sides.south] = false,
  [sides.west] = true,
  [sides.east] = true
})

print("修改前顶部状态：", previous[sides.top])
```

## 相关内容

- `modem`
- `tunnel`
