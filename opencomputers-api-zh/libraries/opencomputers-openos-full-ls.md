# msft

## 概述

供增强版 OpenOS `ls` 命令使用的内部 Microsoft 风格统计摘要辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_ls.lua`

## 用法

```lua
-- internal helpers injected into the extended `ls` implementation
```

## 接口

### msft.final()

- 说明: 在所有目录列表输出完成后，打印按文件系统分组的最终统计摘要。

```lua
msft.final()
```

### msft.report(files, dirs, used, proxy)

- 说明: 为某个文件系统代理打印一段 Microsoft 风格的 `File(s)` 和 `Dir(s)` 统计信息。
- 参数:
  - `files`: 统计到的普通文件数量。
  - `dirs`: 统计到的目录数量。
  - `used`: 当前列表代表的总文件字节数。
  - `proxy`: 用来查询剩余空间和总空间的文件系统代理。

```lua
msft.report(12, 3, 4096, proxy)
```

### msft.tail(names)

- 说明: 为一个已经列完的目录累计并打印统计信息。
- 参数:
  - `names`: 某个目录对应的 `ls` 条目集合表。

```lua
msft.tail(entries)
```

## 备注

- 本页只列出模块导出的公开函数。
