# io

## 概述

从内置 Lua 源码中核对出的 `io` 导出辅助函数。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\17_io.lua`

## 用法

```lua
local io = require("io")
```

## 接口

### io.close(file)

- 说明: 关闭传入的文件句柄；如果未提供，则关闭当前输出流。
- 参数:
  - `file`

```lua
io.close(nil)
```

### io.flush()

- 说明: 刷新当前输出流。

```lua
io.flush()
```

### io.input(file)

- 说明: 获取或替换当前线程的标准输入流，并接受文件对象或路径字符串作为新值。
- 参数:
  - `file`

```lua
io.input(nil)
```

### io.lines(filename, ...)

- 说明: 返回一个迭代器，用来从指定文件或当前输入流中读取记录。
- 参数:
  - `filename`
  - `...`

```lua
io.lines("value", nil)
```

### io.open(path, mode)

- 说明: 通过 Plan9k VFS 打开一个路径，并把得到的流包装成带缓冲的文件对象。
- 参数:
  - `path`
  - `mode`

```lua
io.open(nil, nil)
```

### io.output(file)

- 说明: 获取或替换当前线程的标准输出流，并接受文件对象或路径字符串作为新值。
- 参数:
  - `file`

```lua
io.output(nil)
```

### io.pipe()

- 说明: 返回一对基于 Plan9k `buffer` 模块构建的缓冲输入/输出管道端点。

```lua
local input, output = io.pipe()
```

### io.popen(prog, mode, ...)

- 说明: 以管道端点连接的方式启动一个程序，并根据所选模式返回读句柄、写句柄或二者兼有的句柄。
- 参数:
  - `prog`
  - `mode`
  - `...`

```lua
io.popen(nil, nil, nil)
```

### io.read(...)

- 说明: 使用 Lua 风格格式参数从当前输入流中读取数据。
- 参数:
  - `...`

```lua
io.read(nil)
```

### io.tmpfile()

- 说明: 通过 `os.tmpname()` 创建一个临时文件，并以追加模式打开它。

```lua
io.tmpfile()
```

### io.type(object)

- 说明: 判断某个值是已打开文件对象、已关闭文件对象，还是根本不是文件对象。
- 参数:
  - `object`

```lua
io.type(nil)
```

### io.write(...)

- 说明: 把值写入当前输出流。
- 参数:
  - `...`

```lua
io.write(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
