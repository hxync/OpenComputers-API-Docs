# buffer

## 概述

为缓冲流补充可定位读取、格式化读取和缓冲写入能力的延迟加载扩展。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\lib\core\full_buffer.lua`

## 用法

```lua
local buffer = require("buffer")  -- advanced methods load lazily
```

## 接口

### buffer.buffered_write(arg)

- 说明: 按照当前缓冲策略写入一个字符串，并在需要时先执行刷新。
- 参数:
  - `arg`: 要追加到缓冲区或直接刷到底层流的字符串块。

```lua
stream:buffered_write('hello\n')
```

### buffer.formatted_read(readChunk, ...)

- 说明: 使用 Lua 风格的数字格式和 `*l/*L/*a/*n` 格式说明符读取一个或多个值。
- 参数:
  - `readChunk`: 用于填充内部读取缓冲区的底层分块读取回调。
  - `...`: 格式说明符，或数字形式的字节/字符读取长度。

```lua
local line = stream:formatted_read(readChunk, '*l')
```

### buffer.getTimeout()

- 说明: 返回当前配置的读取超时时间（秒）；若未启用则为 `nil`。

```lua
local timeout = stream:getTimeout()
```

### buffer.readAll(readChunk)

- 说明: 一直读取到 EOF，并把缓冲中的数据与新取到的数据合并后作为一个字符串返回。
- 参数:
  - `readChunk`: 底层分块读取回调。

```lua
local body = stream:readAll(readChunk)
```

### buffer.readBytesOrChars(readChunk, n)

- 说明: 按当前是否为二进制模式，精确读取指定数量的字节或 Unicode 字符。
- 参数:
  - `readChunk`: 底层分块读取回调。
  - `n`: 要读取的字节数或字符数。

```lua
local chunk = stream:readBytesOrChars(readChunk, 16)
```

### buffer.readNumber(readChunk)

- 说明: 跳过前导空白后，解析并返回下一个可读取的数字 token。
- 参数:
  - `readChunk`: 底层分块读取回调。

```lua
local number = stream:readNumber(readChunk)
```

### buffer.seek(whence, offset)

- 说明: 在需要时先刷新待写缓冲，调整读取缓冲状态，然后对底层流执行定位。
- 参数:
  - `whence`: 定位模式：`set`、`cur` 或 `end`。
  - `offset`: 相对于所选模式的整数偏移量。

```lua
stream:seek('set', 0)
```

### buffer.setTimeout(value)

- 说明: 设置缓冲流的读取超时时间（秒）。
- 参数:
  - `value`: 会先经过 `tonumber` 转换的超时值。

```lua
stream:setTimeout(2)
```

### buffer.size()

- 说明: 返回当前通过缓冲包装器可见的未读大小，其中包含内部缓冲中的数据。

```lua
local remaining = stream:size()
```

## 备注

- 本页只列出模块导出的公开函数。
