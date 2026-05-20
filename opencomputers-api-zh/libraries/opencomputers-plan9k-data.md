# data

## 概述

用于二进制与十六进制互转的辅助库，并透传访问主数据卡组件。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\lib\data.lua`

## 用法

```lua
local data = require("data")
```

## 接口

### data.fromHex(hex)

- 说明: 把十六进制文本转换成原始二进制字节。
- 参数:
  - `hex`

```lua
data.fromHex(nil)
```

### data.toHex(data)

- 说明: 把原始二进制字节转换成大写十六进制文本。
- 参数:
  - `data`

```lua
data.toHex(nil)
```

## 备注

- 除本页列出的辅助函数外，未知字段访问会继续透传到 `component.data`，因此数据卡提供的其他方法仍然可以通过这个模块调用。
