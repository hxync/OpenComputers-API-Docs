# du

## 概述

汇总文件大小和目录的递归占用量。

## 可用性

- 仓库: `opencomputers`
- 系统: `plan9k`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\plan9k\bin\du.lua`

## 语法

```lua
du [OPTIONS] [FILE...]
```

## 用途

输出每个文件参数的大小，或者递归计算每个目录树的总占用。遍历目录时不会跟进符号链接的目标内容，符号链接本身按大小 `0` 处理。

## 参数

- `FILE...`: 可选，要查看的文件或目录。省略时默认使用当前目录。

## 选项

- `-h, --human-readable`: 使用 `K`、`M`、`G` 之类的紧凑单位输出大小。
- `-s, --summarize`: 对每个命令行参数只输出一条总计结果。
- `--help`: 输出内置帮助文本。
- `--version`: 输出内置版本信息。

## 示例

```lua
du
```
递归输出当前目录的占用情况。

```lua
du -h /home
```
以易读单位输出 `/home` 的占用情况。

```lua
du -s /tmp /var
```
只为 `/tmp` 和 `/var` 各输出一条总计结果。

## 备注

- 只要有任意路径不存在，命令就会立刻报错并终止。
- 这个实现只接受 `-h`、`-s`、`--help` 和 `--version`；其他选项都会被拒绝。
