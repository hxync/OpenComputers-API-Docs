# irc

## 概述

通过全屏终端客户端连接 IRC 服务器并进行聊天。

## 可用性

- 仓库: `opencomputers`
- 系统: `opencomputers`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\irc\usr\bin\irc.lua`

## 语法

```lua
irc NICKNAME [SERVER[:PORT]]
```

## 用途

通过网络卡建立 TCP 连接，使用给定昵称完成注册，在全屏滚动界面中显示服务器消息，并把终端最后一行保留给用户输入，同时把常见的斜杠命令转换成 IRC 协议消息。

## 参数

- `NICKNAME`: 建立连接时通过 `NICK` 和 `USER` 命令发送的昵称。
- `SERVER[:PORT]`: 可选，IRC 服务器地址。默认值是 `irc.esper.net:6667`。

## 选项

- `--motd`: 显示服务器 MOTD，而不是像默认行为那样略过它们。

## 示例

```lua
irc mynick
```
使用昵称 `mynick` 连接默认服务器。

```lua
irc mynick irc.libera.chat:6667
```
连接到指定的 IRC 服务器和端口。

## 备注

- 客户端内置的交互命令包括 `/msg`、`/join`、`/me`、`/lua`，以及任何形如 `/COMMAND ...` 的原始 IRC 命令。
- 当收到的消息正文包含你当前的昵称时，客户端会发出蜂鸣声提醒。
