# rack_capacitor

## 概述

`rack_capacitor` 是 Computronics 提供的机架式能量缓冲组件。它把这块板卡自身存储的电量暴露给 Lua，适合在脚本里监控机架供电余量。

这是一个只读电源组件，不提供充电、放电或模式切换之类的回调。

## 可用性

- 仓库：`computronics`
- 组件名：`rack_capacitor`
- 常见宿主：安装在机架中的 Computronics 电容板

## 用法

```lua
local component = require("component")
local capacitor = component.rack_capacitor

if not capacitor then
  error("未安装 rack_capacitor 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 接口

### energy

- 语法：`capacitor.energy()`
- 返回值：`number`
- 用途：返回该电容板当前在本地 OpenComputers 缓冲中存储的能量。

这个数值使用的是组件网络缓冲内部采用的能量单位。

示例：

```lua
print("当前存储能量:", capacitor.energy())
```

### maxEnergy

- 语法：`capacitor.maxEnergy()`
- 返回值：`number`
- 用途：返回该电容板可存储的最大能量。

默认配置下容量是 `7500`，但整合包或服务器配置可能会修改这个值。

示例：

```lua
local current = capacitor.energy()
local max = capacitor.maxEnergy()
print(string.format("电容电量: %.0f / %.0f", current, max))
```

## 示例

```lua
local current = capacitor.energy()
local max = capacitor.maxEnergy()

if current < max * 0.25 then
  print("机架电容电量偏低")
end
```

## 相关内容

- `component`
- `light_board`
- `switch_board`
