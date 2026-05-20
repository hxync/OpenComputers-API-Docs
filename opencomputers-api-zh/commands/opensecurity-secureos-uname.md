# uname

## 概述

输出 SecureOS 版本字符串，并可选附带内置内核版本。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\uname.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\uname`

## 语法

```lua
uname
```
```lua
uname -a
```

## 用途

默认输出 `_OSVERSION`。使用 `-a` 时，会继续附加 `_VERSION`，从而同时显示面向用户的操作系统版本和内置内核/运行时版本。

## 选项

- `-a`: 同时输出操作系统版本和内置内核/运行时版本。

## 示例

```lua
uname
```
只输出当前 SecureOS 版本字符串。

```lua
uname -a
```
输出 SecureOS 版本以及内置内核/运行时版本。
