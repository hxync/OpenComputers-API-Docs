# filesystem

## 概述

`filesystem` 组件是 OpenComputers 的标准文件与目录接口。它提供卷标、文件元数据查询、目录操作，以及基于句柄的文件读写能力。

和底层的 `drive` 组件相比，`filesystem` 是更高层、也更适合大多数 Lua 程序直接使用的存储接口。

## 可用性

- 仓库：`opencomputers`
- 组件名：`filesystem`
- 常见宿主：硬盘、软盘、RAID 卷、临时文件系统、挂载式存储

## 重要说明

- 路径会在内部被规范化处理，试图通过 `..` 跳出文件系统根目录的路径会被拒绝。
- `"/"` 和 `"."` 都会被当成文件系统根目录。
- 文件句柄属于打开它的那台电脑。拿别的机器打开的句柄来用，会触发 `"bad file descriptor"`。
- 支持的打开模式只有：
  - `"r"` / `"rb"`
  - `"w"` / `"wb"`
  - `"a"` / `"ab"`

## 用法

```lua
local component = require("component")
local fs = component.filesystem

if not fs then
  error("当前环境没有 filesystem 组件")
end
```

如果要操作特定磁盘，使用 `component.proxy(address)` 更稳妥。

## 卷信息

### getLabel

- 语法：`fs.getLabel()`
- 返回值：`string` 或 `nil`
- 用途：读取卷标。

### setLabel

- 语法：`fs.setLabel(value)`
- 返回值：`string`
- 用途：修改卷标，并返回最终生效的标签。

源码里可能抛出的错误是 `"drive does not support labeling"`。

### isReadOnly

- 语法：`fs.isReadOnly()`
- 返回值：`boolean`
- 用途：判断当前文件系统是否只读。

### spaceTotal

- 语法：`fs.spaceTotal()`
- 返回值：`number`
- 用途：读取文件系统总容量，单位为字节。

如果底层后端报告的是无限容量，源码会返回正无穷。

### spaceUsed

- 语法：`fs.spaceUsed()`
- 返回值：`number`
- 用途：读取当前已使用容量，单位为字节。

## 路径查询

### exists

- 语法：`fs.exists(path)`
- 返回值：`boolean`
- 用途：判断指定路径是否存在。

### size

- 语法：`fs.size(path)`
- 返回值：`number`
- 用途：读取指定路径对应文件或目录项的大小。

### isDirectory

- 语法：`fs.isDirectory(path)`
- 返回值：`boolean`
- 用途：判断指定路径是否为目录。

### lastModified

- 语法：`fs.lastModified(path)`
- 返回值：`number`
- 用途：读取指定路径对应对象的现实时间修改时间戳。

### list

- 语法：`fs.list(path)`
- 返回值：`table` 或 `nil`
- 用途：列出某个目录中的名称列表。

如果该目录不存在，或底层后端无法列出内容，返回值会是 `nil`。

示例：

```lua
local entries = fs.list("/")
if entries then
  for name in entries do
    print(name)
  end
end
```

## 目录与路径修改

### makeDirectory

- 语法：`fs.makeDirectory(path)`
- 返回值：`boolean`
- 用途：创建目录，并在需要时顺带创建缺失的父目录。

行为说明：

- 创建成功时返回 `true`；
- 如果路径已经存在，或者创建失败，则返回 `false`。

### remove

- 语法：`fs.remove(path)`
- 返回值：`boolean`
- 用途：删除文件，或递归删除整个目录树。

这个实现会先进入子目录删除子项，再删除父目录本身。

### rename

- 语法：`fs.rename(from, to)`
- 返回值：`boolean`
- 用途：重命名或移动一个路径对象。

## 文件句柄操作

### open

- 语法：`fs.open(path[, mode])`
- 返回值：`userdata`
- 用途：打开一个文件句柄。

参数说明：

- `path:string`：要打开的路径。
- `mode:string` 可选：默认值是 `"r"`。

行为说明：

- 未知模式会抛出 `"unsupported mode"`；
- 如果当前电脑超过配置允许的最大句柄数，会抛出 `"too many open handles"`；
- 返回值是 userdata 句柄，后续读写和关闭都要继续使用它。

示例：

```lua
local handle = fs.open("/hello.txt", "w")
fs.write(handle, "hello\n")
fs.close(handle)
```

### close

- 语法：`fs.close(handle)`
- 返回值：没有直接返回值
- 用途：关闭一个已经打开的文件句柄。

源码里常见错误是 `"bad file descriptor"`。

### read

- 语法：`fs.read(handle, count)`
- 返回值：`string` 或 `nil`
- 用途：从文件句柄中最多读取 `count` 字节数据。

参数说明：

- `handle:userdata`
- `count:number`

行为说明：

- 到达 EOF 时返回 `nil`；
- 读取大小会被限制在配置允许的最大读取缓冲区内；
- 常见错误包括：
  - `"bad file descriptor"`
  - `"not enough energy"`

### seek

- 语法：`fs.seek(handle, whence, offset)`
- 返回值：`number`
- 用途：修改文件指针位置，并返回新的位置。

参数说明：

- `handle:userdata`
- `whence:string`：只能是 `"set"`、`"cur"`、`"end"`；
- `offset:number`

未知的 `whence` 值会抛出 `"invalid mode"`。

### write

- 语法：`fs.write(handle, value)`
- 返回值：`boolean`
- 用途：向一个已打开的文件句柄写入字节数据。

参数说明：

- `handle:userdata`
- `value:string`

行为说明：

- 成功时返回 `true`；
- 常见错误包括：
  - `"bad file descriptor"`
  - `"not enough energy"`

## 文件读写示例

```lua
local handle = fs.open("/note.txt", "w")
fs.write(handle, "OpenComputers\n")
fs.close(handle)

handle = fs.open("/note.txt", "r")
print(fs.read(handle, 64))
fs.close(handle)
```

## 相关内容

- `drive`
- `disk_drive`
- `component`
