# nc

## 概述

把终端输入输出桥接到内置软件网络栈的 TCP 套接字。

## 可用性

- 仓库: `opencomputers`
- 系统: `network`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\bin\nc.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\network\data\usr\man\nc`

## 语法

```lua
nc PORT ADDRESS
```
```lua
nc -l PORT
```

## 用途

可在客户端模式下连接到远端地址和端口，也可在监听模式下等待一个传入的 TCP 连接。收到的 TCP 数据会直接写到终端，而键盘释放事件产生的字符会逐字节回送到套接字。

## 参数

- `PORT`: 要连接或监听的 TCP 端口。
- `ADDRESS`: 客户端模式下要连接的远端网络地址。

## 选项

- `-l`: 进入监听模式，等待一个传入连接，而不是主动发起外连。

## 示例

```lua
nc 1234 relay-01
```
连接到远端地址 `relay-01` 的 TCP `1234` 端口。

```lua
nc -l 1234
```
在本地 TCP `1234` 端口上监听，直到有对端连入。

## 备注

- 程序会一直运行到被中断为止，没有单独实现退出或断开子命令。
- 输入来自 `key_up` 事件，因此这里只提供非常轻量的终端输入行为，没有完整的行编辑。
