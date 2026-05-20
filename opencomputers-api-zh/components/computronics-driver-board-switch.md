# switch_board

## 概述

`switch_board` 是 Computronics 提供的机架式四路开关输入板。它既可以通过 Lua 切换，也可以直接在机架前面板上点击操作，非常适合做简单控制台、机房拨动开关和手动启停面板。

每个开关都可以单独控制，并且在状态变化时发送 `computer.signal` 事件。

## 可用性

- 仓库：`computronics`
- 组件名：`switch_board`
- 常见宿主：安装在机架中的 Computronics 开关板

## 用法

```lua
local component = require("component")
local switches = component.switch_board

if not switches then
  error("未安装 switch_board 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 开关布局

- 这块板固定提供 `4` 个开关。
- Lua 索引从 `1` 开始。
- 每个保持开启的开关都会在每个游戏刻消耗维护能量。
- 如果机架无法继续提供维护能量，已开启的开关会被自动关闭。

当开关因为玩家点击或 `setActive` 调用而发生状态变化时，板卡会发送以下 `computer.signal` 事件参数：

```lua
"switch_flipped", index, state
```

## 接口

### setActive

- 语法：`switches.setActive(index, active)`
- 返回值：`boolean`
- 用途：把某个开关设置为指定状态。

参数说明：

- `index:number`：开关索引，范围为 `1` 到 `4`。
- `active:boolean`：期望状态。

返回行为：

- 如果状态实际发生变化，返回 `true`
- 如果该开关本来就已经是目标状态，返回 `false`

错误情况：

- 非法索引会抛出 `index out of range: N`

示例：

```lua
local changed = switches.setActive(2, true)
print("状态是否改变:", changed)
```

### isActive

- 语法：`switches.isActive(index)`
- 返回值：`boolean`
- 用途：返回指定开关当前是否为开启状态。

参数说明：

- `index:number`：开关索引，范围为 `1` 到 `4`。

错误情况：

- 非法索引会抛出 `index out of range: N`

示例：

```lua
for i = 1, 4 do
  print(i, switches.isActive(i))
end
```

## 示例

等待玩家手动拨动或脚本切换开关：

```lua
local event = require("event")

while true do
  local _, _, _, name, index, state = event.pull("computer.signal")
  if name == "switch_flipped" then
    print("开关", index, "现在是", state and "开启" or "关闭")
  end
end
```

## 说明

- 该板没有“读取开关数量”的回调，因为数量始终固定为 4。
- 脚本设置和玩家手动点击都会触发同一个 `switch_flipped` 事件。

## 相关内容

- `component`
- `light_board`
- `rack_capacitor`
