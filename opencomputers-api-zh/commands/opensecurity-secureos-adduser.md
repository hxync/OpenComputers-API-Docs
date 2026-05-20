# adduser

## 概述

创建一个 SecureOS 用户账户，并可选授予管理员权限。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\adduser.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\adduser`

## 语法

```lua
adduser USERNAME PASSWORD [true|false]
```
```lua
adduser
```

## 用途

要求 root 权限，然后既可以从命令行读取新用户名和密码，也可以进入交互式提示。脚本会把用户名转成小写，创建 `/home/<username>/` 及其标准子目录，通过 `auth.addUser` 保存账户，并把本次操作写入用户日志。

## 参数

- `USERNAME`: 要创建的账户名称。实现会在保存前把它转换成小写。
- `PASSWORD`: 新账户的初始密码。
- `true|false`: 可选的管理员权限标志。`true` 表示授予管理员权限；省略时默认是 `false`。

## 示例

```lua
adduser steve hunter2 true
```
创建用户 `steve`，把密码设为 `hunter2`，并授予管理员权限。

```lua
adduser
```
打开交互式账户创建提示。

## 备注

- 非 root 用户无法执行这个命令。
- 交互模式会通过 `Y/yes` 或 `N/no` 询问是否授予 root 权限。
