# pwd

## 概述

输出当前工作目录，并可选解析成物理真实路径。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\pwd.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\pwd`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\pwd`

## 语法

```lua
pwd
```
```lua
pwd -P
```

## 用途

把 shell 当前工作目录写到标准输出。使用 `-P` 时，会先调用 `filesystem.realPath` 解析路径，从而折叠符号链接和挂载别名，输出物理目标路径。

## 选项

- `-P`: 输出完全解析后的物理路径，而不是 shell 记录的逻辑路径。

## 示例

```lua
pwd
```
输出 shell 当前记录的逻辑工作目录。

```lua
pwd -P
```
先把当前目录解析到文件系统中的真实目标，再输出该路径。

## 备注

- 如果 `filesystem.realPath` 无法解析当前目录，命令会输出错误并以非零状态退出。
