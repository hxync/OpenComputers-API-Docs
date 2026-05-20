# opencomputers-trade

## 概述

本页说明由 `trading` 组件返回的 `Trade` userdata。每个 `Trade` 对象都表示“当前扫描到的某个村民或商人身上的某一条具体交易配方”。

## 可用性

- 仓库：`opencomputers`
- 运行时类型：`Trade` userdata
- 创建来源：`trading.getTrades()`

## 它表示什么

- 某一个具体商人。
- 该商人的某一条具体交易配方。
- 一次实时可用的交易机会。

这是一个动态对象，因此在后续调用时可能失效，例如：

- 商人已经走远。
- 商人已经死亡或被移除。
- 该配方因为使用次数达到上限而暂时不可用。

## API

### getMerchantId

- 语法：`trade.getMerchantId()`
- 返回值：`number`
- 用途：返回当前 `getTrades()` 扫描结果中用于分组该商人的编号。
- 说明：
  - 这个编号只在当前扫描结果里有意义。
  - 它适合把多条配方归并到同一个商人名下显示。
  - 它不是世界中的永久实体 ID。

### getInput

- 语法：`trade.getInput()`
- 返回值：`firstStack, secondStack`
- 用途：返回这条配方要求支付给商人的输入物品。
- 说明：
  - `firstStack` 始终是主输入。
  - 如果该配方只需要一种输入，`secondStack` 会是 `nil`。
  - 每个返回值都是标准物品堆描述表，包含名称、数量、伤害值、NBT 等常见字段。

### getOutput

- 语法：`trade.getOutput()`
- 返回值：`table`
- 用途：返回这条配方会给出的输出物品。
- 说明：
  - 返回的是标准物品堆描述表。
  - 你可以先读取输出，再决定是否执行交易。

### isEnabled

- 语法：`trade.isEnabled()`
- 返回值：`boolean`
- 用途：返回该交易当前是否仍然可执行。
- 说明：
  - 当商人失效、超出范围或配方暂时关闭时，会返回 `false`。
  - 在执行 `trade.trade()` 之前先调用它，可以减少无效尝试。

### trade

- 语法：`trade.trade()`
- 返回值：
  - 成功：`true`
  - 失败：`false, reason`
- 用途：尝试使用宿主主库存中的物品执行这条交易。

常见失败原因：

- `trading requires an inventory upgrade to be installed`
- `trade has become invalid`
- `out of range`
- `trade is disabled`
- `not enough inventory space to trade`
- `not enough items to trade`

行为说明：

- 宿主必须能够访问自己的主库存。
- 系统会先确认输出物品能否放入库存，再真正扣除输入材料。
- 内部会先尝试更精确的库存提取方式，必要时再使用更宽松的匹配方式重试一次。

## 使用示例

```lua
local component = require("component")
local trading = component.trading

for _, offer in ipairs(trading.getTrades()) do
  if offer.isEnabled() then
    local inputA, inputB = offer.getInput()
    local output = offer.getOutput()

    print("Merchant group:", offer.getMerchantId())
    print("Primary cost:", inputA and inputA.label, inputA and inputA.size)
    print("Secondary cost:", inputB and inputB.label, inputB and inputB.size)
    print("Output:", output and output.label, output and output.size)

    local ok, reason = offer.trade()
    print("Trade result:", ok, reason)
  end
end
```

### 示例用途

适合用于自动村民交易、批量兑换流程、交易列表界面，以及按输出物品筛选最优交易方案。

## 相关内容

- `trading`
- `robot`
- `inventory_controller`
