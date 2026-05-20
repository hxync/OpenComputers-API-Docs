# trading

## 概述

`trading` 组件是交易升级暴露出的顶层接口。它负责发现附近商人的交易配方，并把这些配方包装成 `Trade` userdata 对象返回给 Lua。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`trading`
- 常见宿主：安装了交易升级的机器人、无人机，以及其他代理类宿主

## 作用

- 扫描配置范围内的商人实体，例如村民。
- 为每一条商人配方创建一个 `Trade` userdata 对象。
- 为同一个商人在本次扫描中分配稳定的排序编号，便于把同一商人的多个报价归组。

## 范围规则

- 只有位于 OpenComputers 配置交易范围内的商人才会被纳入结果。
- 扫描区域以宿主当前位置为中心。
- 只有实现了 `IMerchant` 的实体才会被返回。

## API

### getTrades

- 语法：`trading.getTrades()`
- 返回值：`table`
- 用途：返回当前范围内所有商人报价，每一项都是 `Trade` userdata 对象。

返回表会先按商人身份排序，再按该商人的配方顺序排序。

## 返回的 Trade 对象

返回数组中的每一项都是一个 `Trade` userdata 对象，详细接口见配套页面：

- [opencomputers-trade.md](opencomputers-trade.md)

## 示例

```lua
local component = require("component")
local trading = component.trading

for index, offer in ipairs(trading.getTrades()) do
  local first, second = offer.getInput()
  local output = offer.getOutput()
  print(index, offer.getMerchantId(), output and output.name)
end
```

## 相关内容

- `opencomputers-trade`
- `robot`
- `drone`
