# example

## 概述

一个 OpenOS `rc` 服务示例脚本，用于打印欢迎消息并统计自身被启动的次数。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `example`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\etc\rc.d\example.lua`

## 备注

- 这个脚本定义了供 `rc` 服务管理器调用的 `start(msg)` 入口。
- 每次运行时，它都会输出说明文字、来自 `/etc/rc.cfg` 的可选启动消息、当前调用计数，以及当前计算机运行级别。
