# text

## 概述

面向 shell 与终端文本的字符串分词、换行、填充和转义辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_text.lua`

## 用法

```lua
local text = require("text")
```

## 接口

### text.detab(value, tabWidth)

- 说明: 按给定或默认制表宽度把 Tab 字符替换成空格。
- 参数:
  - `value`
  - `tabWidth`

```lua
text.detab(nil, nil)
```

### text.padLeft(value, length)

- 说明: 在字符串左侧补空格，直到达到要求的显示宽度。
- 参数:
  - `value`
  - `length`

```lua
text.padLeft(nil, nil)
```

### text.padRight(value, length)

- 说明: 在字符串右侧补空格，直到达到要求的显示宽度。
- 参数:
  - `value`
  - `length`

```lua
text.padRight(nil, nil)
```

### text.split(input, delimiters, dropDelims, di)

- 说明: 按照给定的分隔符列表拆分字符串，并可选保留分隔符本身。
- 参数:
  - `input`
  - `delimiters`
  - `dropDelims`
  - `di`

```lua
text.split(nil, nil, nil, nil)
```

### text.tokenize(value, options)

- 说明: 把类似 shell 的输入拆分成规范化 token，并正确处理引号和转义。
- 参数:
  - `value`
  - `options`

```lua
text.tokenize(nil, nil)
```

### text.wrap(value, width, maxWidth)

- 说明: 从字符串里取出下一段符合显示宽度限制的行片段，并返回剩余内容。
- 参数:
  - `value`
  - `width`
  - `maxWidth`

```lua
text.wrap(nil, nil, nil)
```

### text.wrappedLines(value, width, maxWidth)

- 说明: 返回一个迭代器，持续调用 `wrap` 来生成换行后的各行文本。
- 参数:
  - `value`
  - `width`
  - `maxWidth`

```lua
text.wrappedLines(nil, nil, nil)
```

## 备注

- OpenOS 会先暴露一个较小的引导期子集，随后在完整模块加载后再补上更丰富的分词和换行工具。
