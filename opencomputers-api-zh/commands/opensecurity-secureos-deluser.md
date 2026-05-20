# deluser

## 概述

删除一个 SecureOS 用户账户，并移除其家目录。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\deluser.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\deluser`

## 语法

```lua
deluser USERNAME
```
```lua
deluser
```

## 用途

要求 root 权限，并从命令行或交互式提示中取得用户名。脚本会把用户名转成小写，通过 `auth.rmUser` 删除账户、记录删除日志，并在 `/home/<username>/` 存在时连同家目录一起移除。

## 参数

- `USERNAME`: 要删除的账户名称。实现会先把它转换成小写。

## 示例

```lua
deluser steve
```
删除账户 `steve`，并在 `/home/steve/` 存在时把它一并删除。

```lua
deluser
```
打开交互式账户删除提示。

## 备注

- 非 root 用户无法执行这个命令。
