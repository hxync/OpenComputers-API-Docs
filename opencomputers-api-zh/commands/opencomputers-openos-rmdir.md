# rmdir

## 概述

删除空目录，并可选地继续删除它们的空父目录。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\rmdir.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\rmdir`

## 语法

```lua
rmdir [OPTIONS] DIRECTORY...
```

## 用途

仅在目录为空时删除每个目录参数。使用 `-p` 时，命令会继续向上尝试删除同一路径链中的空父目录。

## 参数

- `DIRECTORY...`: 一个或多个要删除的目录，这些目录必须事先为空。

## 选项

- `-q, --ignore-fail-on-non-empty`: 抑制“目录非空”的诊断输出，但退出状态仍会记录为失败。
- `-p, --parents`: 同时删除同一路径链中的空父目录。
- `-v, --verbose`: 为每个处理过的目录输出一条诊断信息。
- `--help`: 输出内置帮助文本。

## 示例

```lua
rmdir cache
```
如果 `cache` 为空，则将其删除。

```lua
rmdir -p a/b/c
```
先删除 `a/b/c`，然后在条件满足时继续删除 `a/b` 和 `a`。

## 备注

- 字面路径 `.` 会始终被视为非法参数并拒绝。
- `-q` 只会隐藏“目录非空”的错误消息，并不会把这种情况变成成功。
