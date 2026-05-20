# component

## 概述

带有组件组可见性限制和主组件跟踪能力的 Plan9k `component` 包装层。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\16_component.lua`

## 用法

```lua
local component = require("component")
```

## 接口

### component.get(address, componentType)

- 说明: 解析一个缩写组件地址，并可选把搜索范围限制在某种组件类型内。
- 参数:
  - `address`
  - `componentType`

```lua
component.get("address", nil)
```

### component.getPrimary(componentType)

- 说明: 返回某种组件类型当前的主代理，并在需要时延迟创建它。
- 参数:
  - `componentType`

```lua
component.getPrimary(nil)
```

### component.isAvailable(componentType)

- 说明: 返回请求的组件类型当前是否至少有一个可见实例可用。
- 参数:
  - `componentType`

```lua
component.isAvailable(nil)
```

### component.isPrimary(address)

- 说明: 返回某个组件地址是否是其类型当前的主实例。
- 参数:
  - `address`

```lua
component.isPrimary("address")
```

### component.setPrimary(componentType, address)

- 说明: 切换某种组件类型的主实例，并在合适时触发可用性信号。
- 参数:
  - `componentType`
  - `address`

```lua
component.setPrimary(nil, "address")
```

## 备注

- 本页只列出模块导出的公开函数。
