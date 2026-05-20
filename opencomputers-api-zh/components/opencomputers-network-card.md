# modem

## 概述

本页说明的是标准 OpenComputers 有线网卡暴露出的 `modem` 组件。它允许机器打开端口、检查端口状态、向指定地址发送数据包，以及在可达的有线网络中广播数据包。

旧的生成页里常把它写成 `opencomputers-network-card`，但 Lua 里真正能看到和使用的组件名是 `modem`。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`modem`
- 常见宿主：有线网络卡、机架网络接口、暴露了有线 modem 实现的设备

## 重要说明

- 合法端口范围是 `1..65535`。
- 打开的端口太多时会抛出 `"too many open ports"`。
- 当所连接的电脑启动或停止时，已打开端口会被清空。
- 这张有线卡会返回 `isWireless() == false`，`isWired() == true`。

## 用法

```lua
local component = require("component")
local modem = component.modem

if not modem then
  error("当前环境没有 modem 组件")
end
```

## 接收数据包

当本机在某个端口上已经打开监听，并收到目标端口匹配的数据包时，会触发 `modem_message` 信号。

常见事件负载形式：

```lua
"modem_message", receiverAddress, senderAddress, port, distance, ...
```

对于纯有线 modem，这里的距离通常是 `0`。

## 接口

### open

- 语法：`modem.open(port)`
- 返回值：`boolean`
- 用途：在本地打开一个端口，用来接收传入数据包。

参数说明：

- `port:number`：端口号，范围 `1..65535`。

返回值说明：

- 新打开成功时返回 `true`；
- 该端口原本就已经打开时返回 `false`。

源码里可能抛出的错误是 `"too many open ports"`。

### close

- 语法：
  - `modem.close()`
  - `modem.close(port)`
- 返回值：`boolean`
- 用途：关闭某个端口；如果不传参数，则关闭所有已打开端口。

返回值说明：

- 不带参数时：如果原本有端口处于打开状态并被清空，则返回 `true`；
- 带端口参数时：如果该端口原本是打开的并已关闭，则返回 `true`。

### isOpen

- 语法：`modem.isOpen(port)`
- 返回值：`boolean`
- 用途：检查本地某个端口当前是否已经打开。

### isWireless

- 语法：`modem.isWireless()`
- 返回值：`boolean`
- 用途：判断这张 modem 是否支持无线传输。

对于本页描述的有线卡，这里返回 `false`。

### isWired

- 语法：`modem.isWired()`
- 返回值：`boolean`
- 用途：判断这张 modem 是否支持有线网络流量。

对于本页描述的有线卡，这里返回 `true`。

### send

- 语法：`modem.send(address, port, data...)`
- 返回值：`boolean`
- 用途：向指定目标节点地址发送一个数据包。

参数说明：

- `address:string`：目标节点地址。
- `port:number`：目标端口，范围 `1..65535`。
- `data...`：负载数据。

行为说明：

- 数据包会通过当前可达的有线网络，或机架邻接网络进行传输；
- 该调用在成功排队发送后返回 `true`。

### broadcast

- 语法：`modem.broadcast(port, data...)`
- 返回值：`boolean`
- 用途：向所有可达、且正在监听指定端口的接收方广播一个数据包。

参数说明：

- `port:number`：目标端口，范围 `1..65535`。
- `data...`：负载数据。

它通常用于实现发现协议或简单的局域网广播通信。

### maxPacketSize

- 语法：`modem.maxPacketSize()`
- 返回值：`number`
- 用途：读取当前配置允许的最大数据包大小。

这是单个数据包经过编码后的硬上限。

## 示例

```lua
local event = require("event")

modem.open(1234)
modem.broadcast(1234, "hello", os.time())

local _, _, from, port, _, message = event.pull("modem_message")
print("来自:", from, "端口:", port, "消息:", message)
```

## 相关内容

- `wireless modem`
- `tunnel`
- `relay`
- `component`
