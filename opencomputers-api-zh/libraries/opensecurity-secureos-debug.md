# debug

## 概述

SecureOS 自带的小型调试与娱乐辅助函数，可做循环计时、抛硬币和掷骰子。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\debug.lua`

## 用法

```lua
local debug = require("debug")
```

## 接口

### debug.benchmark(amt)

- 说明: 执行一个简单的求和循环，并把耗费的 CPU 时间格式化为字符串返回。
- 参数:
  - `amt`

```lua
debug.benchmark(nil)
```

### debug.coinToss(times)

- 说明: 模拟多次抛硬币，并返回最终的正面数和反面数。
- 参数:
  - `times`

```lua
debug.coinToss(nil)
```

### debug.diceRoll(amt,sides,mod)

- 说明: 使用一次随机掷骰、数量乘数和可选修正值，返回一个简单骰子表达式的总结果。
- 参数:
  - `amt`
  - `sides`
  - `mod`

```lua
debug.diceRoll(nil, nil, nil)
```

## 备注

- 本页只列出模块导出的公开函数。
