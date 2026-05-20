# filesystem

## 概述

用于 OpenOS 风格文件系统的路径、挂载与文件句柄工具库。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\05_vfs.lua`

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

### filesystem.concat(pathA, pathB, ...)

- 说明: 拼接多个路径片段，并对结果做规范化处理。
- 参数:
  - `pathA`
  - `pathB`
  - `...`

```lua
filesystem.concat(nil, nil, nil)
```

### filesystem.copy(fromPath, toPath)

- 说明: 把文件或目录树复制到新路径。
- 参数:
  - `fromPath`
  - `toPath`

```lua
filesystem.copy(nil, nil)
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

### filesystem.isAutorunEnabled()

- 说明: 返回是否启用了自动执行 autorun 条目。

```lua
filesystem.isAutorunEnabled()
```

### filesystem.isDirectory(path)

- 说明: 检查路径是否指向目录。
- 参数:
  - `path`

```lua
filesystem.isDirectory(nil)
```

### filesystem.isLink(path)

- 说明: 返回路径当前是否解析为符号链接。
- 参数:
  - `path`

```lua
filesystem.isLink(nil)
```

### filesystem.lastModified(path)

- 说明: 返回路径的最后修改时间戳。
- 参数:
  - `path`

```lua
filesystem.lastModified(nil)
```

### filesystem.link(target, linkpath)

- 说明: 创建一个符号链接条目。
- 参数:
  - `target`
  - `linkpath`

```lua
filesystem.link(nil, nil)
```

### filesystem.list(path)

- 说明: 列出目录中的条目；适用时也会包含虚拟挂载点条目。
- 参数:
  - `path`

```lua
filesystem.list(nil)
```

### filesystem.makeDirectory(path)

- 说明: 创建目录，并在实现支持时补齐所需的中间结构。
- 参数:
  - `path`

```lua
filesystem.makeDirectory(nil)
```

### filesystem.mount(fs, path)

- 说明: 把文件系统代理挂载到指定的虚拟路径。
- 参数:
  - `fs`
  - `path`

```lua
filesystem.mount(nil, nil)
```

### filesystem.mounts()

- 说明: 遍历当前已挂载的文件系统及其挂载点。

```lua
filesystem.mounts()
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

### filesystem.proxy(filter)

- 说明: 把文件系统地址或筛选条件解析为文件系统代理。
- 参数:
  - `filter`

```lua
filesystem.proxy(nil)
```

### filesystem.remove(path)

- 说明: 删除文件、目录，或可移除的虚拟条目。
- 参数:
  - `path`

```lua
filesystem.remove(nil)
```

### filesystem.rename(oldPath, newPath)

- 说明: 把条目重命名或移动到新路径。
- 参数:
  - `oldPath`
  - `newPath`

```lua
filesystem.rename(nil, nil)
```

### filesystem.segments(path)

- 说明: 把路径拆分成规范化后的片段列表。
- 参数:
  - `path`

```lua
filesystem.segments(nil)
```

### filesystem.setAutorunEnabled(value)

- 说明: 开启或关闭自动执行 autorun 条目。
- 参数:
  - `value`

```lua
filesystem.setAutorunEnabled(nil)
```

### filesystem.size(path)

- 说明: 返回文件大小，单位为字节。
- 参数:
  - `path`

```lua
filesystem.size(nil)
```

### filesystem.umount(fsOrPath)

- 说明: 按代理、地址或挂载路径卸载文件系统。
- 参数:
  - `fsOrPath`

```lua
filesystem.umount(nil)
```

## 备注

- 本页只记录导出的 `filesystem.*` 函数。
- 路径分段、挂载树遍历等内部辅助函数被刻意排除在公开 API 列表之外。
