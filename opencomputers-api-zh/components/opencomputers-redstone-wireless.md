# opencomputers-redstone-wireless

## 概述

本页记录高级 `redstone` 组件在接入受支持的无线红石集成后额外获得的无线红石回调接口。

## 可用性

- 仓库：`opencomputers`
- 实际 Lua 组件名：`redstone`
- 底层实现特征：`RedstoneWireless`
- 外部依赖：受支持的无线红石模组，例如 WR-CBE

## 作用

- 跟踪一个无线红石频率。
- 读取当前调谐频率上是否有无线红石输入。
- 启用或关闭本设备自身的无线发射状态。
- 把设备重新调到另一个无线频率。

## 信号行为

无线输入变化也会转换成 OpenComputers 使用的同一种红石信号：

```lua
"redstone_changed", "wireless", oldValue, newValue
```

在无线变化场景下，第二个参数不是数字侧面，而是字面字符串 `"wireless"`。

## API

### getWirelessInput

- 语法：`redstone.getWirelessInput()`
- 返回值：`boolean`
- 用途：轮询当前配置频率上的无线输入状态。

尽管部分自动生成文档会把它写成数字，实际实现返回的是布尔电平状态。

### getWirelessOutput

- 语法：`redstone.getWirelessOutput()`
- 返回值：`boolean`
- 用途：检查当前设备是否正在发射有电的无线信号。

### setWirelessOutput

- 语法：`redstone.setWirelessOutput(value)`
- 参数：
  - `value:boolean`：目标发射状态。
- 返回值：`boolean`
- 用途：修改无线发射状态，并返回修改前的旧状态。

### getWirelessFrequency

- 语法：`redstone.getWirelessFrequency()`
- 返回值：`number`
- 用途：读取当前配置的无线频率。

### setWirelessFrequency

- 语法：`redstone.setWirelessFrequency(frequency)`
- 参数：
  - `frequency:number`：新的无线频率。
- 返回值：`number`
- 用途：修改无线频率，并返回修改前的旧频率。

重要副作用：

- 重新调频后，缓存的无线输入会被清空，同时无线输出也会被强制重置为 `false`。

## 使用说明

- 修改频率时，调用上下文会短暂停顿。
- 当组件从网络中断开时，它会注销自己的无线接收器和发射器，并把已保存频率重置为 `0`。
- 无线唤醒仍然通过统一的 `redstone_changed` 与唤醒阈值逻辑工作。

## 示例

```lua
local component = require("component")
local redstone = component.redstone

print("Old frequency:", redstone.setWirelessFrequency(1234))
print("Input powered:", redstone.getWirelessInput())

local oldOutput = redstone.setWirelessOutput(true)
print("Previous transmit state:", oldOutput)
print("Current transmit state:", redstone.getWirelessOutput())
```

## 相关内容

- `opencomputers-redstone-vanilla`
- `opencomputers-redstone-signaller`
