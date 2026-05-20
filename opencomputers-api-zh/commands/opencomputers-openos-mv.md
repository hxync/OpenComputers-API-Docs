# mv

## 概述

移动或重命名文件、链接和整个目录树。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mv.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mv`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mv`

## 语法

```lua
mv [OPTIONS] SOURCE... DEST
```

## 用途

把一个或多个源路径移动到新的目标位置。该实现复用了共享传输库，会保留符号链接、允许移动目录，并在复制成功后删除原始路径。

## 参数

- `SOURCE...`: 一个或多个要移动的文件、链接或目录。
- `DEST`: 目标文件路径，或接收结果的目标目录。

## 选项

- `-f`: 直接覆盖已存在的文件，不进行提示。
- `-i`: 覆盖已存在目标文件前先提示确认；如果同时给出 `-f`，则不再提示。
- `-n`: 如果目标文件已存在，则不覆盖。
- `-v`: 在执行移动时输出每一步操作。
- `--skip=PATTERN`: 跳过解析后完整路径匹配该 Lua 模式的源路径。
- `-h, --help`: 输出内置帮助文本。

## 示例

```lua
mv a.txt b.txt
```
把 `a.txt` 重命名或移动到 `b.txt`。

```lua
mv a.txt b.txt /tmp
```
把 `a.txt` 和 `b.txt` 一起移动到 `/tmp`。

```lua
mv -n /home/data /backup/data
```
只有在 `/backup/data` 不存在时才移动 `/home/data`。

## 备注

- 挂载点不能被 `mv` 移动。
- 如果源路径以 `.` 或 `..` 结尾，命令会将其视为非法的移动源并拒绝执行。
