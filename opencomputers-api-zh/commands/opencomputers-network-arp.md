# arp

## 概述

查看内置 MCNET 软件网络栈维护的 ARP 地址解析表。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\bin\arp.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\arp`

## 语法

```lua
arp
```

## 用途

输出当前本机已知的地址解析条目，并标明每条记录属于哪个可见网络接口。命令会先从 `network.info.getInfo().interfaces` 读取接口列表，再对每个接口调用 `network.info.getArpTable(interface)`，最后把结果整理成一个两列表格。

## 输出

- `Address`: 本地 MCNET 栈已经解析出的主机地址。
- `Iface`: 学到该地址时所对应的接口名称。

## 示例

```lua
arp
```
显示当前所有可见 MCNET 接口上的地址解析表。

## 备注

- 即使当前没有任何 ARP 条目，脚本仍然会先打印 `Address` / `Iface` 表头。
- 条目会按 Lua `pairs` 返回的接口顺序分组显示，因此不同运行之间的显示顺序不保证完全稳定。
