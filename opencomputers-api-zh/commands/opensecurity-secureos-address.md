# address

## 概述

输出当前电脑的组件地址。

## 可用性

- 仓库: `opensecurity`
- 系统: `secureos`
- 类型: `command`

## 来源

- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\bin\address.lua`
- `OpenSecurity/src\main\resources\assets\opensecurity\lua\SecureOS\usr\man\address`
- `OpenComputers/src\main\resources\assets\opencomputers\loot\openos\usr\man\address`

## 语法

```lua
address
```

## 用途

把这台机器自身的组件地址写到标准输出。当你需要拿这个地址做联网或远程识别、又手头没有分析仪时，这个命令非常方便。

## 示例

```lua
address
```
显示当前执行该命令的电脑地址。
