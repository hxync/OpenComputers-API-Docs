# ec

## 概述

本页说明 OpenComputers 向 Lua 暴露的 Extra Cells 集成接口。它会在 AE 网络相关组件上额外增加气体列表读取能力。

## 可用性

- 依赖: `ec`
- 标签: `integration-required`

## 组件名

- `me_controller`
- `me_interface`

## 主要用途

- 列出 Extra Cells AE 网络中存储的气体。
- 在已有 AE 控制器或接口自动化脚本上增加气体感知能力。

## 组件

### me_controller

当启用 `ec` 集成后，`me_controller` 会额外获得一个和气体相关的接口。

#### `getGasesInNetwork()`

- 语法: `getGasesInNetwork(): table`
- 用途: 返回 AE 网络中当前存储的全部气体栈。
- 返回值:
  - 由多个气体描述表组成的 Lua 数组。
- 说明:
  - 只有被 Extra Cells 识别为气体的流体条目才会返回。
  - 这个接口是对普通 `appeng` 网络查询接口的补充。

### me_interface

方块形态的 `me_interface` 在连接到 Extra Cells 网络时，也会获得同样的气体接口。

#### `getGasesInNetwork()`

- 语法: `getGasesInNetwork(): table`
- 用途: 返回当前连接 AE 网络里可见的全部气体栈。
- 返回值:
  - 气体描述表数组。
- 说明:
  - 如果你的自动化逻辑是围绕接口方块而不是控制器展开，这个接口会更顺手。

## 示例

```lua
local component = require("component")
local me = component.me_controller

for _, gas in ipairs(me.getGasesInNetwork()) do
  print(gas.label or gas.name, gas.amount)
end
```

## 相关内容

- `appeng`
- `mekanism`
