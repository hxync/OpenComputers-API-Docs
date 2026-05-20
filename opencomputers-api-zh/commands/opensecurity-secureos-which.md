# which

## 概述

定位命令名对应的解析路径或别名目标。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\which.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\which`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\which`

## 语法

```lua
which COMMAND
```

## 用途

按 shell 的搜索路径解析每个给出的命令名。如果它不是实际文件，但存在对应别名，则会改为报告别名指向的目标。

## 参数

- `COMMAND`: 要解析的命令名称。

## 示例

```lua
which ls
```
输出 `ls` 的完整路径。

```lua
which doesntexist dir
```
对 `doesntexist` 显示未找到错误，并对 `dir` 显示别名展开结果。

## 备注

- 当查找失败时，命令会把错误写到标准错误输出，并以失败状态退出。
