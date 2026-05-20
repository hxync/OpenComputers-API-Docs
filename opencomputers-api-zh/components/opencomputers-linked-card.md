# tunnel

## 概述

本页说明的是链接卡提供的 `tunnel` 组件。链接卡是一种点对点或共享频道式的远距离通信设备，可以绕过普通局域网络的距离限制。

旧生成页里经常把它写成 `opencomputers-linked-card`，但 Lua 里真正可见的组件名是 `tunnel`。

## 可用性

- 仓库：`opencomputers`
- Lua 组件名：`tunnel`
- 常见宿主：链接卡

## 频道模型

每张链接卡都属于一个共享频道字符串。源码里保存这个频道字段时使用的名字就是 `tunnel`。

同一频道上的卡可以互相交换数据包：

- 数据包会发给该频道上除自己以外的所有端点；
- 发送方不会收到自己发出的数据包。

## 用法

```lua
local component = require("component")
local tunnel = component.tunnel

if not tunnel then
  error("当前环境没有 tunnel 组件")
end
```

## 接收消息

链接卡收到的数据本质上也是通过组件网络层投递到机器上的，因此实际脚本里通常仍然会通过事件监听方式来处理它。

## 接口

### send

- 语法：`tunnel.send(data...)`
- 返回值：
  - 成功：`true`
  - 失败：`nil, reason`
- 用途：向同一频道上的其他所有链接卡发送一个数据包。

参数说明：

- `data...`：数据包负载。

行为说明：

- 发送方不会收到自己发出的包；
- 传输距离不像普通无线 modem 那样受范围限制；
- 发送会消耗相对更高的能量。

源码中的失败原因是 `"not enough energy"`。

### maxPacketSize

- 语法：`tunnel.maxPacketSize()`
- 返回值：`number`
- 用途：读取当前配置允许的最大数据包大小。

### getChannel

- 语法：`tunnel.getChannel()`
- 返回值：`string`
- 用途：读取当前这张链接卡所属的共享频道标识。

示例：

```lua
print("链接频道:", tunnel.getChannel())
tunnel.send("sync", os.time())
```

## 相关内容

- `modem`
- `wireless modem`
- `component`
