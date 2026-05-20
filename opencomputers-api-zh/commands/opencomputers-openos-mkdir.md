# mkdir

## 概述

创建一个或多个目录。

## 可用性

- 仓库: `opencomputers`
- 系统: `openos`
- 类型: `command`

## 来源

- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\bin\mkdir.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\mkdir`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\mkdir`

## 语法

```lua
mkdir DIRECTORY...
```

## 用途

创建每个给定的目录路径。命令会先结合当前 shell 工作目录解析参数，再调用文件系统库创建目录。

## 参数

- `DIRECTORY...`: 一个或多个要创建的目录路径。

## 示例

```lua
mkdir a
```
在当前目录创建 `a`。

```lua
mkdir /a/b c
```
如果不存在，则先创建 `/a`，再创建 `/a/b`，并在当前目录创建 `c`。

## 备注

- 创建失败时，命令会输出解析后的目标路径以及文件系统返回的错误原因。
