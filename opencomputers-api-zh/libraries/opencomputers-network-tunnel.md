# tunnel

## 概述

一个建立在 OpenComputers 隧道卡之上的软件隧道网络驱动，并复用与调制解调器后端相同的发现协议。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\tunnel.lua`

## 用法

```lua
local tunnel = require("network.tunnel")
```

## 接口

### tunnel.info(interface)

- 说明: 返回某个由隧道卡支撑的接口（例如 `tun0`）的收发包数和字节数统计。
- 参数:
  - `interface`

```lua
tunnel.info(0)
```

### tunnel.send(handle, interface, destination, data)

- 说明: 通过隧道接口发送负载；如果目标就是同一张隧道卡，则直接在本地短路处理。
- 参数:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
tunnel.send(nil, 0, nil, nil)
```

### tunnel.start(eventHandler)

- 说明: 扫描隧道组件，注册 `tun*` 接口，发送发现包，并挂接共享的 `modem_message` 监听器。
- 参数:
  - `eventHandler`

```lua
tunnel.start(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
