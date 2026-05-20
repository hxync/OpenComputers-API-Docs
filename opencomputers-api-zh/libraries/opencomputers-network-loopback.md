# loopback

## 概述

一个本地回环网络驱动，会把数据包直接回送给内置的软件网络栈。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\loopback.lua`

## 用法

```lua
local loopback = require("network.loopback")
```

## 接口

### loopback.info(interface)

- 说明: 返回回环接口的收发包数和字节数统计。
- 参数:
  - `interface`

```lua
loopback.info(0)
```

### loopback.send(handle, interface, destination, data)

- 说明: 当目标是接口 `lo` 上的 `localhost` 时，把数据包交给回环处理器。
- 参数:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
loopback.send(nil, 0, nil, nil)
```

### loopback.start(eventHandler)

- 说明: 使用给定事件处理器注册 `lo` 接口以及对应的 `localhost` 主机条目。
- 参数:
  - `eventHandler`

```lua
loopback.start(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
