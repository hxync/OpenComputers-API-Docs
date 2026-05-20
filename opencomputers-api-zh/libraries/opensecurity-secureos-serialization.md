# serialization

## 概述

把 Lua 值转换成近似源码的文本形式，并再还原回 Lua 值。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `library`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\lib\serialization.lua`

## 用法

```lua
local serialization = require("serialization")
```

## 接口

### serialization.serialize(value, pretty)

- 说明: 把数字、字符串、布尔值、nil 和表转换成近似 Lua 字面量的文本，并可选启用美化格式。
- 参数:
  - `value`
  - `pretty`

```lua
serialization.serialize(nil, nil)
```

### serialization.unserialize(data)

- 说明: 在受限环境中执行序列化文本，并返回重建后的 Lua 值。
- 参数:
  - `data`

```lua
serialization.unserialize(nil)
```

## 备注

- 美化输出模式更适合阅读展示，但它生成的文本不一定还能被 `unserialize` 无损还原。
