# filesystem

## 概述

用于 OpenOS 风格文件系统的路径、挂载与文件句柄工具库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\filesystem.lua`

## 用法

```lua
local filesystem = require("filesystem")
```

## 接口

### filesystem.canonical(path)

- 说明: 规范化路径，折叠多余的分隔符和相对路径片段。
- 参数:
  - `path`

```lua
filesystem.canonical(nil)
```

### filesystem.concat(...)

- 说明: 拼接多个路径片段，并对结果做规范化处理。
- 参数:
  - `...`

```lua
filesystem.concat(nil)
```

### filesystem.exists(path)

- 说明: 检查路径是否存在。
- 参数:
  - `path`

```lua
filesystem.exists(nil)
```

### filesystem.get(path)

- 说明: 把路径解析为底层文件系统代理，以及该文件系统内部的相对路径。
- 参数:
  - `path`

```lua
filesystem.get(nil)
```

### filesystem.isDirectory(path)

- 说明: 检查路径是否指向目录。
- 参数:
  - `path`

```lua
filesystem.isDirectory(nil)
```

### filesystem.list(path)

- 说明: 列出目录中的条目；适用时也会包含虚拟挂载点条目。
- 参数:
  - `path`

```lua
filesystem.list(nil)
```

### filesystem.mount(fs, path)

- 说明: 把文件系统代理挂载到指定的虚拟路径。
- 参数:
  - `fs`
  - `path`

```lua
filesystem.mount(nil, nil)
```

### filesystem.name(path)

- 说明: 返回路径最后一个片段。
- 参数:
  - `path`

```lua
filesystem.name(nil)
```

### filesystem.open(path, mode)

- 说明: 打开文件，并返回一个把句柄操作转发到底层文件系统的包装对象。
- 参数:
  - `path`
  - `mode`

```lua
filesystem.open(nil, nil)
```

### filesystem.path(path)

- 说明: 返回路径中的目录部分。
- 参数:
  - `path`

```lua
filesystem.path(nil)
```

### filesystem.proxy(filter, options)

- 说明: 把文件系统地址或筛选条件解析为文件系统代理。
- 参数:
  - `filter`
  - `options`

```lua
filesystem.proxy(nil, nil)
```

### filesystem.realPath(path)

- 说明: 解析符号链接和挂载边界，得到实际落到文件系统上的路径。
- 参数:
  - `path`

```lua
filesystem.realPath(nil)
```

## 备注

- 本页只记录导出的 `filesystem.*` 函数。
- 路径分段、挂载树遍历等内部辅助函数被刻意排除在公开 API 列表之外。
