# buffer

## 概述

供 OpenOS 与 SecureOS I/O 封装层使用的带缓冲类文件流辅助库。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\buffer.lua`

## 用法

```lua
local buffer = require("buffer")
```

## 接口

### buffer.close()

- 说明: 在需要时先刷新待写缓冲，标记包装对象已关闭，然后关闭底层流。

```lua
buffer.close()
```

### buffer.flush()

- 说明: 把当前待写输出缓冲立即写入底层流。

```lua
buffer.flush()
```

### buffer.lines(...)

- 说明: 返回一个迭代器，用来持续从带缓冲流中读取记录。
- 参数:
  - `...`

```lua
buffer.lines(nil)
```

### buffer.new(mode, stream)

- 说明: 把原始流句柄包装成带缓冲、带读取超时状态且具备读写模式标记的对象。
- 参数:
  - `mode`
  - `stream`

```lua
buffer.new(nil, nil)
```

### buffer.read(...)

- 说明: 按 Lua 风格格式参数读取数据；如果没有给格式，就按读一行处理。
- 参数:
  - `...`

```lua
buffer.read(nil)
```

### buffer.readLine(chop, timeout)

- 说明: 从内部读取缓冲中读出一行，并可选择是否保留行结束符。
- 参数:
  - `chop`
  - `timeout`

```lua
buffer.readLine(nil, nil)
```

### buffer.setvbuf(mode, size)

- 说明: 把缓冲模式设置为 `no`、`full` 或 `line`，并可选地调整缓冲区大小。
- 参数:
  - `mode`
  - `size`

```lua
buffer.setvbuf(nil, nil)
```

### buffer.write(...)

- 说明: 把字符串或数字写入流，并遵循当前缓冲模式。
- 参数:
  - `...`

```lua
buffer.write(nil)
```

## 备注

- `buffer.new` 返回的实例会包装底层流，并补上行缓冲、格式化读取、超时控制和类文件辅助方法。
