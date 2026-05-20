# factorization

## 概述

本页说明 OpenComputers 对 Factorization 电荷导体的集成接口。

## 可用性

- 依赖: `factorization`
- 标签: `integration-required`

## 组件名

- `charge_conductor`

## 主要用途

- 监控导体网络中的电荷状态。
- 判断一段导体当前是否带电。
- 为自动化排障记录电荷变化情况。

## 组件

### charge_conductor

`charge_conductor` 组件对应实现了 Factorization `IChargeConductor` 接口的方块。

#### `getCharge()`

- 语法: `getCharge(): number`
- 用途: 返回该方块当前的电荷值。
- 返回值:
  - 当前电荷数值。
- 说明:
  - 如果底层方块当前没有可读取的 charge 对象，这个接口会返回 `0`。
  - 电荷数值的具体量纲取决于被查询的 Factorization 方块类型，因此更适合在同类设备之间做比较，而不是假设它们都使用统一上限。

#### 示例

```lua
local component = require("component")
local conductor = component.charge_conductor

local charge = conductor.getCharge()
print("Charge:", charge)

if charge <= 0 then
  print("Conductor is not carrying charge.")
end
```

## 相关内容

- `redlogic`
