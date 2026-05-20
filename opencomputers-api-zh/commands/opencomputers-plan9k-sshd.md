# sshd

## 概述

监听 TCP 22 端口上的 Plan9k SSH 连接，并为其生成 shell 会话。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\sshd.lua`

## 语法

```lua
sshd
```

## 用途

在 `22` 端口打开 TCP 监听器，等待来自 `network` 的入站连接事件，并为每个成功接入的通道启动 `/usr/sbin/sshsession.lua`，让认证与 shell 转发逻辑在独立进程中执行。

## 示例

```lua
sshd
```
以前台方式启动 Plan9k SSH 监听服务。

## 备注

- 会话处理进程会读取 `/etc/passwd`，并要求其中使用 Plan9k 认证工具所写入的口令格式。
- 这个监听器会一直运行，直到进程被显式终止。
