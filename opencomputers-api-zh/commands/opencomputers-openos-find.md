# find

## 概述

递归查找文件、目录或符号链接。

## 可用性

- 仓库：`opencomputers`
- 系统：`openos`
- 类型：`command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\find.lua`

## 语法

```lua
find [PATH] [--type=d|f|s] [--name=EXPR] [--iname=EXPR]
```

## 用途

遍历目标目录树，并输出所有符合类型条件和可选名称表达式的路径。输出结果会保持与你传入起始路径时相对应的相对路径风格。

## 参数

- `PATH`：可选，起始目录。省略时默认使用当前工作目录。
- `EXPR`：类 glob 的名称表达式，其中 `*` 可匹配任意长度的字符序列。

## 选项

- `--type=d`：只输出目录。
- `--type=f`：只输出普通文件。
- `--type=s`：只输出符号链接。
- `--name=EXPR`：使用当前实现中的第一种名称匹配模式。
- `--iname=EXPR`：使用当前实现中的第二种名称匹配模式。
- `--help`：输出内置用法说明。

## 示例

```lua
find
```

递归列出当前目录下的全部文件系统条目。

```lua
find /etc --type=f
```

只输出 `/etc` 下的普通文件。

```lua
find /home --name='*.lua'
```

在 `/home` 中查找最终名称匹配 `*.lua` 的路径。

## 备注

- 这个实现会把 `*` 转换成 Lua 模式中的通配片段，并要求整个文件名完整匹配。
- 当前实现里，`--name` 实际是大小写不敏感，`--iname` 反而是大小写敏感，这与帮助文本正好相反。
