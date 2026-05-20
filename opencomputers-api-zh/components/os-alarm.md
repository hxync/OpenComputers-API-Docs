# os_alarm

## 概述

`os_alarm` 组件用于控制 OpenSecurity 的警报器方块。它可以启动和停止警报、切换为其他已安装的警报音效、列出可用的警报声音文件，并且在配置允许时还能在世界指定坐标播放任意声音。

## 可用性

- 仓库：`opensecurity`
- 组件名：`os_alarm`
- 常见宿主：放置在世界中的 OpenSecurity 警报器

## 用法

```lua
local component = require("component")
local alarm = component.os_alarm

if not alarm then
  error("未安装 os_alarm 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定设备。

## 声音模型

- 警报器会把当前音效名称保存在 `soundName` 中。
- 默认音效是 `klaxon1`。
- 音量实际保存在浮点值 `volume` 中。
- `setRange` 并不是直接保存你传入的方块距离，而是会换算成 `range / 15 + 0.5`。

## 声音文件说明

`listSounds()` 会返回 `mods/OpenSecurity/sounds/alarms/` 目录中扫描到的原始文件名。

在这个版本里，返回值通常会包含类似 `klaxon1.ogg` 这样的完整文件名，但 `setAlarm(name)` 内部会检查 `name .. ".ogg"` 是否存在。所以实际使用时通常应当：

- 用 `listSounds()` 查看有哪些声音
- 调用 `setAlarm()` 时传入去掉 `.ogg` 后缀的基础名称

## 接口

### greet

- 语法：`alarm.greet()`
- 返回：`string`
- 用途：返回 OpenSecurity 内置的问候语。

固定返回文本：

```text
Lasciate ogne speranza, voi ch'intrate
```

示例：

```lua
print(alarm.greet())
```

### setRange

- 语法：`alarm.setRange(range)`
- 返回：`string`
- 用途：调整警报器的有效可听范围。

参数说明：

- `range:number`：期望设置的方块范围。

行为说明：

- 允许的取值范围是 `15` 到 `150`。
- 设置成功时返回 `"Success"`。
- 超出范围时返回 `"Error, range should be between 15-150"`。

示例：

```lua
print(alarm.setRange(45))
```

### setAlarm

- 语法：`alarm.setAlarm(name)`
- 返回：`string`
- 用途：切换警报器当前使用的音效。

参数说明：

- `name:string`：音效基础名，通常不要带 `.ogg` 后缀。

行为说明：

- 如果 `mods/OpenSecurity/sounds/alarms/<name>.ogg` 存在，则保存该音效并返回 `"Success"`。
- 如果文件不存在，则返回 `"Fail"`。

示例：

```lua
print(alarm.setAlarm("klaxon1"))
```

### activate

- 语法：`alarm.activate()`
- 返回：`string`
- 用途：启动警报音。

行为说明：

- 会把内部 `computerPlaying` 标记设为 `true`。
- 返回 `"Ok"`。

示例：

```lua
alarm.activate()
```

### deactivate

- 语法：`alarm.deactivate()`
- 返回：`string`
- 用途：停止警报音。

行为说明：

- 会把内部 `computerPlaying` 标记设为 `false`。
- 返回 `"Ok"`。

示例：

```lua
alarm.deactivate()
```

### listSounds

- 语法：`alarm.listSounds()`
- 返回：`table`
- 用途：列出扫描到的警报声音文件。

行为说明：

- 该表在模组启动时通过扫描 `mods/OpenSecurity/sounds/alarms/` 目录生成。
- 返回的文件名保持磁盘上的原始名称。

示例：

```lua
for i, name in ipairs(alarm.listSounds()) do
  print(i, name)
end
```

### playSoundAt

- 语法：`alarm.playSoundAt(x, y, z, sound, range)`
- 返回：`string`
- 用途：在指定世界坐标播放任意声音效果。

参数说明：

- `x:number`：世界 X 坐标。
- `y:number`：世界 Y 坐标。
- `z:number`：世界 Z 坐标。
- `sound:string`：要播放的完整 Minecraft 声音 id。
- `range:number`：声音半径参数。源码注释建议使用 `1` 到 `10`。

行为说明：

- 只有在 OpenSecurity 配置中启用了 `playSoundAt` 时该接口才可用。
- 成功时返回 `"Ok"`。
- 若配置禁用，则返回 `"Disabled"`。

示例：

```lua
local result = alarm.playSoundAt(0, 64, 0, "minecraft:random.click", 4)
print(result)
```

## 示例

```lua
local sounds = alarm.listSounds()
if #sounds > 0 then
  local first = sounds[1]:gsub("%.ogg$", "")
  alarm.setAlarm(first)
end

alarm.setRange(60)
alarm.activate()
os.sleep(2)
alarm.deactivate()
```

## 相关内容

- `component`
- `os_energyturret`
