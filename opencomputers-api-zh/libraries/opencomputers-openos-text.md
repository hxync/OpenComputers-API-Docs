# text

## 概述

面向 shell 与终端文本的字符串分词、换行、填充和转义辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\text.lua`

## 用法

```lua
local text = require("text")
```

## 接口

### text.escapeMagic(txt)

- 说明: 转义 Lua 模式里的特殊字符，使字符串能够按字面量匹配。
- 参数:
  - `txt`

```lua
text.escapeMagic(nil)
```

### text.removeEscapes(txt)

- 说明: 把 `escapeMagic` 针对 Lua 模式特殊字符添加的转义去掉。
- 参数:
  - `txt`

```lua
text.removeEscapes(nil)
```

### text.trim(value)

- 说明: 移除字符串首尾的空白字符。
- 参数:
  - `value`

```lua
text.trim(nil)
```

## 备注

- OpenOS 会先暴露一个较小的引导期子集，随后在完整模块加载后再补上更丰富的分词和换行工具。
