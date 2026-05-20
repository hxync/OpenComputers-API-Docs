# useradd

## 概述

把玩家加入电脑的授权用户列表。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\useradd.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\useradd`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\useradd`

## 语法

```lua
useradd NAME
```

## 用途

把指定的已登录玩家加入这台机器的授权用户列表。一旦列表中至少有一个用户，只有列表中的玩家才能与这台电脑交互。

## 参数

- `NAME`: 要授权的玩家精确名称。

## 示例

```lua
useradd Steve
```
授权已登录玩家 `Steve` 使用这台电脑。

## 备注

- 玩家名称区分大小写。
- 之后可以使用 `userdel` 再次移除这些用户。
- 电脑所有权机制可以在配置中关闭。
