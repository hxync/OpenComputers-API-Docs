# io

## 概述

建立在文件系统流之上的高层文件描述符、流和管道辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\io.lua`

## 用法

```lua
local io = require("io")
```

## 接口

### io.close(file)

- 说明: 关闭传入的文件句柄；如果未传入，则关闭当前输出流。
- 参数:
  - `file`

```lua
io.close(nil)
```

### io.dup(fd)

- 说明: 围绕现有文件描述符对象创建一个轻量级副本包装器。
- 参数:
  - `fd`

```lua
io.dup(nil)
```

### io.error(file)

- 说明: 获取或替换当前进程的标准错误流。
- 参数:
  - `file`

```lua
io.error(nil)
```

### io.flush()

- 说明: 刷新当前标准输出流。

```lua
io.flush()
```

### io.input(file)

- 说明: 获取或替换当前进程的标准输入流。
- 参数:
  - `file`

```lua
io.input(nil)
```

### io.lines(filename, ...)

- 说明: 返回一个文件记录迭代器；数据来源可以是指定文件，也可以是当前输入流。
- 参数:
  - `filename`
  - `...`

```lua
io.lines("value", nil)
```

### io.open(path, mode)

- 说明: 打开文件系统路径，并把返回的句柄包装成带缓冲的流对象。
- 参数:
  - `path`
  - `mode`

```lua
io.open(nil, nil)
```

### io.output(file)

- 说明: 获取或替换当前进程的标准输出流。
- 参数:
  - `file`

```lua
io.output(nil)
```

### io.popen(prog, mode, env)

- 说明: 打开通往其他进程或命令管线的管道。
- 参数:
  - `prog`
  - `mode`
  - `env`

```lua
io.popen(nil, nil, nil)
```

### io.read(...)

- 说明: 使用 Lua 风格格式参数从当前标准输入流读取数据。
- 参数:
  - `...`

```lua
io.read(nil)
```

### io.stream(fd,file,mode)

- 说明: 读取或替换当前进程 I/O 表里按数字索引保存的标准流。
- 参数:
  - `fd`
  - `file`
  - `mode`

```lua
io.stream(nil, nil, nil)
```

### io.tmpfile()

- 说明: 通过 `os.tmpname()` 创建并打开一个临时文件。

```lua
io.tmpfile()
```

### io.type(object)

- 说明: 判断某个值是已打开文件、已关闭文件，还是根本不是文件对象。
- 参数:
  - `object`

```lua
io.type(nil)
```

### io.write(...)

- 说明: 把值写入当前标准输出流。
- 参数:
  - `...`

```lua
io.write(nil)
```

## 备注

- OpenOS 与 Plan9k 都提供兼容风格的 `io` 表，并通过每进程句柄状态来路由标准流。
