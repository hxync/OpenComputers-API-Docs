# mktmp

## 概述

创建一个带随机名称的临时文件或目录。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mktmp.lua`

## 语法

```lua
mktmp [OPTIONS] [PATH]
```

## 用途

通过 `os.tmpname()` 生成一个路径，并在该位置创建空文件或目录。默认会在 `$TMPDIR` 下创建；如果标准输出连接到 TTY，则会自动打印生成出的路径。

## 参数

- `PATH`: 可选，要在其下创建临时条目的目录前缀。省略时使用 `$TMPDIR/`。

## 选项

- `-d`: 创建目录，而不是文件。
- `-v, --verbose`: 始终打印生成出的路径。
- `-q, --quiet`: 除非同时使用 `-v`，否则不打印生成出的路径。
- `--help`: 输出内置帮助文本。

## 示例

```lua
mktmp
```
在 `$TMPDIR` 下创建一个临时文件。

```lua
mktmp -d -v /tmp
```
在 `/tmp` 下创建临时目录，并打印生成出的路径。

## 备注

- 这个实现实际是通过 shell 解析后的普通 `touch` 和 `mkdir` 命令来完成创建动作。
- 如果选定的前缀路径本身不存在，命令会在报告 `os.tmpname()` 结果前直接失败。
