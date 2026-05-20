# leash

## 概述

`leash` 组件是 OpenComputers 的拴绳升级。它可以把附近的可拴系生物拴到宿主设备上，也可以一次性释放当前由这个升级拴住的全部实体。

## 可用性

- 仓库：`opencomputers`
- 组件名：`leash`
- 常见宿主：装有拴绳升级的兼容 OpenComputers 设备

## 用法

```lua
local component = require("component")
local leash = component.leash
```

## 限制

- 最多同时跟踪 `8` 个被拴住的实体
- 只会处理 `allowLeashing() == true` 的生物实体
- 当组件断开、宿主卸载或显式调用 `unleash()` 时，当前跟踪到的实体都会被释放

## 接口

### leash

- 语法：`leash.leash(side)`
- 返回：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：沿指定方向搜索第一个可拴系的生物实体，并把它拴到宿主上。

参数说明：

- `side:number`：任意合法的 OpenComputers 方向值。

行为说明：

- 搜索区域从宿主包围盒开始，朝指定方向延伸两格。
- 成功时当前上下文会暂停 `0.1` 秒。

常见失败：

- `nil, "too many leashed entities"`
- `nil, "no unleashed entity"`

示例：

```lua
local ok, reason = leash.leash(sides.front)
if not ok then
  print(reason)
end
```

### unleash

- 语法：`leash.unleash()`
- 返回：无可依赖的有效返回值
- 用途：释放当前由这个升级跟踪的全部实体。

行为说明：

- 实现会在宿主周围扩展 `5` 格的范围内搜索，并清除那些确实是由该升级拴上的实体。

## 相关内容

- `component`
- `drone`
