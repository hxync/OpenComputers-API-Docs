# modem

## 概述

这张 Computronics 伪装卡虽然暴露出来的组件名仍然是 `modem`，但它允许 Lua 在 `send` 和 `broadcast` 时伪造数据包的源地址。

它适合做协议测试、数据包模拟、伪装设备仿真，以及调试那些依赖特定源地址流量的软件。

## 可用性

- 仓库：`computronics`
- 组件名：`modem`
- 常见宿主：安装了 Computronics 伪装网卡的设备

## 用法

```lua
local component = require("component")
local modem = component.modem

if not modem then
  error("未安装 modem 组件")
end
```

也可以使用 `component.proxy(address)` 访问指定地址的设备。

## 重要行为

- 这个组件继承了普通 OpenComputers 有线 `modem` 的标准 API。
- 在标准网卡回调基础上，它的 `send` 和 `broadcast` 额外支持可选的伪造源地址参数。
- 每次伪造发送都会额外消耗伪装卡能量。
- 端口号必须位于 `1` 到 `65535` 之间。

由于它继承自标准网卡实现，因此也支持普通调制解调器的常见回调，例如：

- `open`
- `close`
- `isOpen`
- `isWireless`
- `isWired`
- `maxPacketSize`

这些继承来的回调行为与普通有线 `modem` 一致。

## 接口

### send

- 语法：`modem.send(targetAddress[, sourceAddress], port, data...)`
- 返回值：
  - 成功：`true`
  - 失败：`false`
- 用途：向指定目标发送数据包，并可选地伪装成另一个源地址。

参数说明：

- `targetAddress:string`：目标组件地址。
- `sourceAddress:string` 可选：要伪造写入数据包中的源地址。
- `port:number`：目标端口，范围 `1` 到 `65535`。
- `data...`：负载数据。

行为说明：

- 如果省略 `sourceAddress`，就使用组件真实地址。
- 如果组件没有足够的伪装能量，接口返回 `false`。
- 非法端口会抛出 `invalid port number`。

示例：使用真实源地址正常发送。

```lua
assert(modem.send("12345678-1234-1234-1234-123456789abc", 42, "ping"))
```

示例：伪造源地址发送。

```lua
assert(modem.send(
  "12345678-1234-1234-1234-123456789abc",
  "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  42,
  "spoofed payload"
))
```

### broadcast

- 语法：`modem.broadcast([sourceAddress], port, data...)`
- 返回值：
  - 成功：`true`
  - 失败：`false`
- 用途：在指定端口广播数据包，并可选地使用伪造源地址。

参数说明：

- `sourceAddress:string` 可选：写入数据包中的伪造源地址。
- `port:number`：广播端口，范围 `1` 到 `65535`。
- `data...`：负载数据。

行为说明：

- 如果省略 `sourceAddress`，使用组件真实地址。
- 如果组件没有足够的伪装能量，接口返回 `false`。
- 非法端口会抛出 `invalid port number`。

示例：

```lua
assert(modem.broadcast(100, "status", true))
```

示例：广播时伪装发送者。

```lua
assert(modem.broadcast(
  "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee",
  100,
  "simulated-node-online"
))
```

## 说明

- 这一页重点描述的是伪装扩展行为；继承来的端口管理行为可以直接按标准有线 OpenComputers `modem` 来理解。
- 接收数据包仍然依赖继承实现中的开端口逻辑。

## 相关内容

- `component`
- `opencomputers-network-card`
