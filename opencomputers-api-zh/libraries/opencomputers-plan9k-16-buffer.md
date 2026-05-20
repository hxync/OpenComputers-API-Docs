# buffer

## 概述

Plan9k 的缓冲流辅助库，其中包含供伪终端和新建程序使用的内存管道对。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\lib\modules\base\16_buffer.lua`

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

- 说明: 立即把当前待写输出缓冲写入被包装的底层流。

```lua
buffer.flush()
```

### buffer.getTimeout()

- 说明: 返回这条缓冲流当前配置的读取超时时间，单位为秒。

```lua
buffer.getTimeout()
```

### buffer.lines(...)

- 说明: 返回一个迭代器，按给定读取格式持续从缓冲流中读取记录。
- 参数:
  - `...`

```lua
buffer.lines(nil)
```

### buffer.new(mode, stream)

- 说明: 把一个原始流句柄包装成带有 Plan9k 缓冲状态、读缓冲、写缓冲和模式标记的对象。
- 参数:
  - `mode`
  - `stream`

```lua
buffer.new(nil, nil)
```

### buffer.pipe()

- 说明: 创建一对相连的缓冲读端和写端，底层由内存队列以及线程唤醒信号驱动。

```lua
local input, output = buffer.pipe()
```

### buffer.read(...)

- 说明: 在遵守当前超时配置的前提下，使用 Lua 风格读取格式从底层流读取数据。
- 参数:
  - `...`

```lua
buffer.read(nil)
```

### buffer.seek(whence, offset)

- 说明: 按需刷新写缓冲，调整读取缓冲状态，并重新定位被包装的底层流。
- 参数:
  - `whence`
  - `offset`

```lua
buffer.seek(nil, nil)
```

### buffer.setTimeout(value)

- 说明: 设置后续缓冲读取要使用的超时时间，单位为秒。
- 参数:
  - `value`

```lua
buffer.setTimeout(nil)
```

### buffer.setvbuf(mode, size)

- 说明: 设置后续写入要使用的缓冲模式和缓冲区大小。
- 参数:
  - `mode`
  - `size`

```lua
buffer.setvbuf(nil, nil)
```

### buffer.write(...)

- 说明: 在遵守当前缓冲模式的前提下，通过缓冲包装器写入字符串或数字。
- 参数:
  - `...`

```lua
buffer.write(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
