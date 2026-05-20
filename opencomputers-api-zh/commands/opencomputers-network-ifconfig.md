# ifconfig

## 概述

查看 MCNET 接口状态，并为当前主机绑定额外的本地地址。

## 可用性

- 仓库：`opencomputers`
- 系统：`network`
- 类型：`command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\ifconfig.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\ifconfig`

## 语法

```lua
ifconfig
```

```lua
ifconfig bind ADDRESS
```

## 用途

不带参数时，命令会列出当前检测到的每个 MCNET 接口摘要，包括链路类型、硬件地址、收发包数量以及收发字节数。

带上 `bind ADDRESS` 时，命令会把给定地址附加到当前主机上，作为额外的本地地址。

## 参数

- `ADDRESS`：要绑定到当前主机上的附加本地地址。

## 输出字段

- `Link encap`：该接口使用的链路层后端名称。
- `HWaddr`：接口的硬件地址或软件地址。
- `RX packets` / `TX packets`：累计接收包数与发送包数。
- `RX bytes` / `TX bytes`：累计接收字节数与发送字节数。

## 示例

```lua
ifconfig
```

输出所有已检测接口的地址信息和流量统计。

```lua
ifconfig bind relay-01
```

把额外本地地址 `relay-01` 绑定到当前节点。

## 备注

- 对于回环接口 `lo`，命令还会额外打印一次电脑组件地址，作为补充硬件地址。
- 这个版本只支持 `bind` 这个修改型子命令，没有实现其他配置子命令。
- `bind` 本身不会先做地址格式预检查，而是直接交给底层网络库处理；如果地址无效或被拒绝，错误会由底层返回。
