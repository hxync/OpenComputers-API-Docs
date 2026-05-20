# ln

## 概述

创建一个虚拟符号链接。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\ln.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\ln`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\ln`

## 语法

```lua
ln FILE [TARGET]
```

## 用途

创建一个指向其他文件或目录路径的文件系统链接。如果没有显式给出新链接的位置，命令会在当前工作目录下使用目标的基础名称创建链接。

## 参数

- `FILE`: 要引用的现有文件、目录或符号链接。
- `TARGET`: 可选，新符号链接的目标路径。

## 示例

```lua
ln /bin/ls.lua
```
在当前工作目录中创建 `ls.lua`，并让它指向 `/bin/ls.lua`。

```lua
ln /home/magic.lua /bin/magic.lua
```
创建 `/bin/magic.lua`，让它链接到 `/home/magic.lua`。

## 备注

- OpenOS 的链接是虚拟条目，重启后不会持久保留。
- 命令允许把一个已损坏的符号链接作为源，但会拒绝一个完全不存在的源路径。
