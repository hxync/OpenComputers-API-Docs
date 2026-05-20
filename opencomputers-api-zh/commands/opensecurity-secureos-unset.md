# unset

## 概述

删除一个或多个环境变量。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\unset.lua`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\unset`

## 语法

```lua
unset [VARNAME]
```

## 用途

命令会对每个给出的变量名调用 `os.setenv(name, nil)`，从而将其从环境中移除。

## 参数

- `VARNAME`: 要删除的环境变量名称。

## 示例

```lua
unset some_variable
```
删除环境变量 `some_variable`。
