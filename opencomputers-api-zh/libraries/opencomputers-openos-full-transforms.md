# transforms

## 概述

叠加在基础 `transforms` 模块之上的扩展序列处理辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_transforms.lua`

## 用法

```lua
local transforms = require("transforms")  -- advanced helpers load lazily
```

## 接口

### transforms.at(tbl, index)

- 说明: 返回在某个 `pairs` 遍历位置上遇到的键值对。
- 参数:
  - `tbl`: 将使用 `pairs` 遍历的表。
  - `index`: 要取出的 1 基键值对序号。

```lua
local key, value = transforms.at(env, 1)
```

### transforms.foreach(tbl,c,f,l)

- 说明: 把回调映射到某个表区间上，并收集所有非 `nil` 的结果。
- 参数:
  - `tbl`: 源索引表。
  - `c`: 回调函数，或者表示要从每个元素上提取字段值的字段名。
  - `f`: 可选，起始索引。
  - `l`: 可选，结束索引。

```lua
local names = transforms.foreach(parts, function(part) return part.txt end)
```

### transforms.partition(tbl,partitioner,dropEnds,f,l)

- 说明: 使用分隔谓词或分隔序列把一个表拆成若干子区间。
- 参数:
  - `tbl`: 源索引表。
  - `partitioner`: 分隔谓词，或分隔序列表。
  - `dropEnds`: 为真时，从返回的分段中省略分隔元素。
  - `f`: 可选，起始索引。
  - `l`: 可选，结束索引。

```lua
local groups = transforms.partition(words, function(word) return word == '|' end, true)
```

### transforms.sub(tbl,f,l)

- 说明: 返回类似 `string.sub` 的表切片，只是操作对象变成了索引表元素。
- 参数:
  - `tbl`: 源索引表。
  - `f`: 可选，起始索引。
  - `l`: 可选，结束索引。

```lua
local head = transforms.sub(words, 1, 3)
```

### transforms.where(tbl,p,f,l)

- 说明: 按谓词过滤某个表区间，并只返回匹配到的元素。
- 参数:
  - `tbl`: 源索引表。
  - `p`: 谓词函数，调用方式为 `p(element, index, table)`。
  - `f`: 可选，起始索引。
  - `l`: 可选，结束索引。

```lua
local quoted = transforms.where(parts, function(part) return part.qr end)
```

## 备注

- 本页只列出模块导出的公开函数。
