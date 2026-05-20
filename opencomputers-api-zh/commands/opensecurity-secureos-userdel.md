# userdel

## 概述

把玩家从电脑的授权用户列表中移除。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\userdel.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\userdel`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\userdel`

## 语法

```lua
userdel NAME
```

## 用途

把指定玩家从这台机器的授权用户列表中移除。

## 参数

- `NAME`: 要从授权列表中移除的玩家精确名称。

## 示例

```lua
userdel Steve
```
把 `Steve` 从授权用户列表中移除。

## 备注

- 如果之后需要重新授权，请使用 `useradd`。
