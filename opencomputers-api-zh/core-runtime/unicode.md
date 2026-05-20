# unicode

## 概述

`unicode` 接口提供按码点处理字符串的工具，以及面向终端显示宽度的辅助函数。

## 可用性

- 仓库: `opencomputers`
- 类型: 核心运行时接口

## 来源

- `OpenComputers/src\main\scala\li\cil\oc\server\machine\luac\UnicodeAPI.scala`

## 用法

```lua
local unicode = require("unicode")
print(unicode.len("hello"))
```

## 接口

### char(...codepoints)

- 说明: 通过一个或多个 Unicode 码点构造字符串。

### len(value)

- 说明: 返回字符串的码点长度。

### lower(value)

- 说明: 返回字符串的小写形式。

### reverse(value)

- 说明: 返回反转后的 Unicode 字符串。

### sub(value, start[, finish])

- 说明: 按码点索引返回子串。

### upper(value)

- 说明: 返回字符串的大写形式。

### isWide(char)

- 说明: 返回首个码点是否为宽字符。

### charWidth(char)

- 说明: 返回首个码点的显示宽度。

### wlen(value)

- 说明: 返回字符串的显示宽度。

### wtrunc(value, count)

- 说明: 按目标显示宽度截断字符串。

## 相关内容

- `computer`
- `component`
- `os`
- `unicode`
