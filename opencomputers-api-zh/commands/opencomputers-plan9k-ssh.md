# ssh

## 概述

通过 TCP 22 端口连接到 Plan9k SSH 服务器并打开加密远程 shell 会话。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\usr\bin\ssh.lua`

## 语法

```lua
ssh ADDRESS
```

## 用途

先提示输入密码，再连接目标地址的 `22` 端口，完成 Plan9k `sshsession.lua` 实现的自定义挑战应答握手，派生会话密钥，然后通过加密 TCP 数据包双向转发本地标准输入与远端 shell 输出。

## 参数

- `ADDRESS`: 传给 `network.tcp.open` 的远程主机地址。

## 示例

```lua
ssh 192.168.1.10
```
连接远程主机 `192.168.1.10`，并启动加密 shell 会话。

## 备注

- 这个命令依赖 Plan9k 的 `network` 栈，以及 `data` 组件提供的随机数、哈希、加密和解密能力。
- 远端必须运行与之配套的 Plan9k `sshd` 服务；它并不兼容 OpenSSH 协议。
