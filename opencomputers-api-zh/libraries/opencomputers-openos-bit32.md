# bit32

## 概述

为兼容性和底层数据处理提供的 Lua 5.2 风格 32 位位运算辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\bit32.lua`

## 用法

```lua
local bit32 = require("bit32")
```

## 接口

### bit32.arshift(x, disp)

- 说明: 对 32 位整数执行算术右移。
- 参数:
  - `x`
  - `disp`

```lua
bit32.arshift(nil, nil)
```

### bit32.band(...)

- 说明: 对所有给定操作数执行按位与。
- 参数:
  - `...`

```lua
bit32.band(nil)
```

### bit32.bnot(x)

- 说明: 对 32 位整数执行按位取反。
- 参数:
  - `x`

```lua
bit32.bnot(nil)
```

### bit32.bor(...)

- 说明: 对所有给定操作数执行按位或。
- 参数:
  - `...`

```lua
bit32.bor(nil)
```

### bit32.btest(...)

- 说明: 返回所有操作数按位与后的结果是否非零。
- 参数:
  - `...`

```lua
bit32.btest(nil)
```

### bit32.bxor(...)

- 说明: 对所有给定操作数执行按位异或。
- 参数:
  - `...`

```lua
bit32.bxor(nil)
```

### bit32.extract(n, field, width)

- 说明: 按给定偏移和宽度提取位字段。
- 参数:
  - `n`
  - `field`
  - `width`

```lua
bit32.extract(nil, nil, nil)
```

### bit32.lrotate(x, disp)

- 说明: 在 32 位值范围内向左循环移位。
- 参数:
  - `x`
  - `disp`

```lua
bit32.lrotate(nil, nil)
```

### bit32.lshift(x, disp)

- 说明: 对位模式执行逻辑左移。
- 参数:
  - `x`
  - `disp`

```lua
bit32.lshift(nil, nil)
```

### bit32.replace(n, v, field, width)

- 说明: 用新值替换 32 位整数中的某个位字段。
- 参数:
  - `n`
  - `v`
  - `field`
  - `width`

```lua
bit32.replace(nil, nil, nil, nil)
```

### bit32.rrotate(x, disp)

- 说明: 在 32 位值范围内向右循环移位。
- 参数:
  - `x`
  - `disp`

```lua
bit32.rrotate(nil, nil)
```

### bit32.rshift(x, disp)

- 说明: 对位模式执行逻辑右移。
- 参数:
  - `x`
  - `disp`

```lua
bit32.rshift(nil, nil)
```

## 备注

- 本页只列出模块导出的公开函数。
