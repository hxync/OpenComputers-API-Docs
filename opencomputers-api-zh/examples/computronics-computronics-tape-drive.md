# tape_drive

## 概述

一个自动运行辅助脚本，用来为 Computronics 的 `tape` 命令注册 shell 自动补全。

## 可用性

- 仓库: `computronics`
- 系统: `computronics`
- 类型: `example`

## 来源

- `Computronics/src\main\resources\assets\computronics\lua\peripheral\tape_drive\autorun\tape_drive`

## 备注

- 它会把 `/rom/programs/tape_drive` 追加到 shell 搜索路径中。
- 注册的补全函数会提示 `play`、`stop`、`pause`、`rewind`、`label`、`speed`、`volume` 和 `write` 等子命令；当第二个参数属于 `write` 时，还会继续调用文件系统路径补全。
