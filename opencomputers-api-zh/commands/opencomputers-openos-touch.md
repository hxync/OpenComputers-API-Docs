# touch

## 概述

更新文件修改时间，或创建空文件。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\touch.lua`

## 语法

```lua
touch [OPTIONS] FILE...
```

## 用途

以追加模式打开每个给定文件，从而把它的修改时间刷新到当前时刻。当路径不存在时，命令会创建一个空文件，除非使用了 `-c` 或 `--no-create`。

## 参数

- `FILE...`: 一个或多个要刷新或创建的文件路径。

## 选项

- `-c, --no-create`: 不要创建原本不存在的文件。
- `--help`: 输出内置帮助文本。

## 示例

```lua
touch note.txt
```
如果 `note.txt` 不存在则创建它；如果已存在则刷新其修改时间。

```lua
touch -c cache.lock
```
只有在 `cache.lock` 已存在时才刷新它。

## 备注

- 目录参数不会被修改，命令只会输出一条忽略警告。
- 当对不存在的路径使用 `--no-create` 时，该参数会被报告为一次失败。
