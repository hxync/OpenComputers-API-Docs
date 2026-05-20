# modem

## 概述

一个建立在 OpenComputers 调制解调器组件和简单发现协议之上的软件以太网风格驱动。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `library`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\lib\network\modem.lua`

## 用法

```lua
local modem = require("network.modem")
```

## 接口

### modem.info(interface)

- 说明: 返回某个由调制解调器支撑的接口（例如 `eth0`）的收发包数和字节数统计。
- 参数:
  - `interface`

```lua
modem.info(0)
```

### modem.send(handle, interface, destination, data)

- 说明: 把负载发送到目标调制解调器地址；如果目标就是本接口自己的调制解调器，则直接在本地短路处理。
- 参数:
  - `handle`
  - `interface`
  - `destination`
  - `data`

```lua
modem.send(nil, 0, nil, nil)
```

### modem.start(eventHandler)

- 说明: 扫描调制解调器组件，注册 `eth*` 接口，打开 `1` 号端口，广播发现包，并挂接 `modem_message` 监听器。
- 参数:
  - `eventHandler`

```lua
modem.start(nil)
```

## 备注

- 本页只列出模块导出的公开函数。
