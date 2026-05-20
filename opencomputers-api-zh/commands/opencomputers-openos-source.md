# source

## 概述

按行在子 shell 进程中执行脚本文件。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\source.lua`

## 语法

```lua
source FILE
```

## 用途

逐行读取一个文件，并为每一行启动一个新的 shell 进程来继续执行。实现会显式共享当前的别名表和 shell 变量表，因此某些环境变化能够回写到当前会话。

## 参数

- `FILE`: 要按行执行的脚本文件。

## 选项

- `-q`: 当文件无法被读取时，安静地抑制打开失败的错误信息。

## 示例

```lua
source /etc/profile
```
通过当前 shell 程序逐行执行 `/etc/profile` 中的内容。

## 备注

- 命令严格要求且只接受一个文件参数。
- 这个实现是“逐行执行”模型，因此它的行为并不等同于传统 shell 那种把整个文件作为一个脚本体统一解析的 `source`。
