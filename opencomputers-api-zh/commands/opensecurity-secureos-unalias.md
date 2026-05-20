# unalias

## 概述

删除一个 SecureOS shell 别名，并输出被删除的映射。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\unalias.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\unalias`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unalias`

## 语法

```lua
unalias NAME
```

## 用途

查找指定别名。若存在，则删除它并输出被移除的 `name -> value` 映射；若不存在，则报告 `no such alias`。

## 参数

- `NAME`: 要删除的别名名称。

## 示例

```lua
unalias ll
```
删除别名 `ll`，并输出它此前指向的目标。
