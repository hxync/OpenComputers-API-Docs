# list

## 概述

以最小开销输出原始目录条目列表。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\list.lua`

## 语法

```lua
list [PATH]
```

## 用途

列出某个目录的直接内容，不附加额外格式化、排序、颜色或元数据列。这个命令的定位是 `ls` 的低内存替代品。

## 参数

- `PATH`: 可选，要列出的目录。省略时默认为 `.`。

## 选项

- `--help`: 输出内置帮助文本。

## 示例

```lua
list
```
按每行一个条目的方式输出当前目录内容。

```lua
list /bin
```
输出 `/bin` 的原始目录内容。

## 备注

- 命令只会使用第一个位置参数。
- 开始列出之前，目标路径必须能够解析成一个已存在的目录。
