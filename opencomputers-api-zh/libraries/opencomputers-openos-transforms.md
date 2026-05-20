# transforms

## 概述

被 OpenOS shell 解析器大量使用的表切片与 token 序列辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\transforms.lua`

## 用法

```lua
local transforms = require("transforms")
```

## 接口

### transforms.begins(tbl,v,f,l)

- 说明: 判断某个表区间是否以另一个索引序列作为开头。
- 参数:
  - `tbl`: 要检查的源索引表。
  - `v`: 应当匹配区间开头的索引序列。
  - `f`: 可选，要测试的起始索引。
  - `l`: 可选，要测试的结束索引。

```lua
transforms.begins(tokens, {'--'})
```

### transforms.concat(...)

- 说明: 把多个索引表拼接成一个结果表；如果输入带有打包用 `n` 字段，也会尽量保留。
- 参数:
  - `...`: 按顺序追加的任意数量索引表。

```lua
local merged = transforms.concat(args, extra)
```

### transforms.first(tbl,pred,f,l)

- 说明: 找到第一个满足谓词的元素或子区间，或者匹配若干候选 token 序列之一的位置。
- 参数:
  - `tbl`: 要扫描的索引表。
  - `pred`: 谓词函数，或候选索引序列表。
  - `f`: 可选，开始扫描的索引。
  - `l`: 可选，结束扫描的索引。

```lua
local i = transforms.first(words, function(part) return part == '|' end)
```

## 备注

- 基础模块只暴露一小组核心功能；像 `partition`、`foreach` 这样的附加能力会从 `full_transforms` 按需挂接。
