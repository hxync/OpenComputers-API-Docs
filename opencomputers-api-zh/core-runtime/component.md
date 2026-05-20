# component

## 概述

`component` 运行时接口用于列出可见组件、检查可调用方法并按地址调用回调。

## 可用性

- 仓库: `opencomputers`
- 类型: 核心运行时接口

## 来源

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\ComponentAPI.scala`

## 用法

```lua
local component = require("component")
for address, name in component.list() do
  print(address, name)
end
```

## 接口

### list([filter[, exact]])

- 说明: 返回可见组件列表，可按类型过滤。

### type(address)

- 说明: 返回指定地址对应的组件类型。

### slot(address)

- 说明: 返回宿主中该组件所在的插槽。

### methods(address)

- 说明: 返回组件的回调元数据。

### invoke(address, method, ...)

- 说明: 调用远程组件上的回调。

### doc(address, method)

- 说明: 返回回调的源码文档字符串。

## 相关内容

- `computer`
- `component`
- `os`
- `unicode`
